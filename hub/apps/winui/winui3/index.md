---
title: WinUI 3 Preview 2 (2020 年 7 月)
description: WinUI 3 Preview 2 リリースの概要。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: c57132ec5219ef32f2b2b69168592e07f49d904b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168776"
---
# <a name="windows-ui-library-3-preview-2-july-2020"></a>Windows UI ライブラリ 3 Preview 2 (2020 年 7 月)

Windows UI ライブラリ (WinUI) 3 は、Windows デスクトップと UWP の両アプリに対応したネイティブ ユーザー エクスペリエンス (UX) フレームワークです。

**WinUI 3 Preview 2** は、Preview 1 リリースのバグと既知の問題の修正に重点を置いた、品質と安定性に基づくリリースです。

**[Preview 2 の制限事項と既知の問題](#preview-2-limitations-and-known-issues)** を参照してください。

> [!Important]
> この WinUI 3 Preview リリースは、早期評価と、開発者コミュニティからのフィードバックの収集を目的としています。 実稼働アプリには使用**できません**。
>
> WinUI 3 Preview のリリースは 2020 年から 2021 年前半にかけて継続され、その後、最初の公式リリースが利用可能になる予定です。
>
> [WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)を使用して、フィードバックを提供し、提案と問題をログに記録してください。

## <a name="install-winui-3-preview-2"></a>WinUI 3 Preview 2 をインストールする

WinUI 3 Preview 2 には、WinUI ベースのユーザー インターフェイスを備えたアプリの構築を始める際に役立つ Visual Studio プロジェクト テンプレートと、WinUI ライブラリを含む NuGet パッケージが含まれています。 WinUI 3 Preview 2 をインストールするには、次の手順を実行します。

> [!NOTE]
> [XAML コントロール ギャラリー](#xaml-controls-gallery-winui-3-preview-2-branch)の WinUI 3 Preview 2 バージョンを複製して構築することもできます。

1. 開発用コンピューターに Windows 10 バージョン 1803 (ビルド 17134) 以降がインストールされていることを確認します。

2. [Visual Studio 2019 バージョン 16.7.2](https://visualstudio.microsoft.com/vs/) をインストールします

    Visual Studio をインストールする際、次のワークロードを含める必要があります。
    - .NET デスクトップ開発
    - ユニバーサル Windows プラットフォームの開発

    C++ アプリを構築するには、次のワークロードも含める必要があります。
    - C++ によるデスクトップ開発
    - ユニバーサル Windows プラットフォーム ワークロード用の *C++ (v142) ユニバーサル Windows プラットフォーム ツール*のオプション コンポーネント (右ペインにある [ユニバーサル Windows プラットフォーム開発] セクションの [インストールの詳細] を参照してください)

    Visual Studio をダウンロードしたら、プログラム内で .NET のプレビューを有効にしてください。 
    - [ツール] > [オプション] > [プレビュー機能] に移動して、[.NET Core SDK のプレビューを使用する (再起動が必要)] を選択します。 

3. C#/.NET 5 および C++/Win32 の各アプリ用のデスクトップ WinUI プロジェクトを作成する場合は、.NET 5 Preview 5 の x64 および x86 の両バージョンもインストールする必要があります。 **.NET 5 Preview 5 は、現在 WinUI 3 でサポートされている唯一の .NET 5 のプレビューであることに注意してください**。

    - x64: [https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe)
    - x86: [https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe)

4. [WinUI 3 Preview 2 VSIX パッケージ](https://aka.ms/winui3/previewdownload)をダウンロードしてインストールします。 この VSIX パッケージをインストールすると、WinUI 3 プロジェクト テンプレートと、WinUI 3 ライブラリを含む NuGet パッケージが Visual Studio 2019 に追加されます。

    VSIX パッケージを Visual Studio に追加する方法については、[Visual Studio 拡張機能を見つけて使用する方法](/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box)に関するページをご覧ください。


## <a name="create-winui-projects"></a>WinUI プロジェクトを作成する

WinUI 3 Preview 2 VSIX パッケージをインストールすると、Visual Studio の WinUI プロジェクト テンプレートのいずれかを使用して新しいプロジェクトを作成できるようになります。 **[新しいプロジェクトの作成]** ダイアログで WinUI プロジェクト テンプレートにアクセスするには、言語を **C++** または **C#** に、プラットフォームを **Windows** に、プロジェクトの種類を **WinUI** にフィルター処理します。 または、*WinUI* を検索して、使用できる C# または C++ テンプレートのいずれかを選択することもできます。

![WinUI プロジェクト テンプレート](images/winui-projects-csharp.png)

WinUI プロジェクト テンプレートの概要については、次の記事を参照してください。

- [デスクトップ アプリ用の WinUI 3 の概要](get-started-winui3-for-desktop.md)
- [UWP アプリ用の WinUI 3 の概要](get-started-winui3-for-uwp.md)

[制限と既知の問題](#preview-2-limitations-and-known-issues)を除くと、WinUI プロジェクトを使用したアプリの構築は、XAML と WinUI 2.x を使用した UWP アプリの構築と似ています。 そのため、Windows SDK の UWP アプリと **Windows.UI** WinRT 名前空間のガイダンスとドキュメントのほとんどを適用できます。

WinUI 3 Preview 1 を使用してプロジェクトを作成した場合、Preview 2 を使用するようにプロジェクトをアップグレードすることができます。 詳細な手順については、[GitHub リポジトリ](https://aka.ms/winui3/upgrade-instructions)を参照してください。

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
| Windows ランタイム コンポーネント (UWP の WinUI) | C++ および C# | C# または C++/WinRT で記述された [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。これは、アプリの記述に使用されたプログラミング言語に関係なく、WinUI ベースのユーザー インターフェイスを備えた任意の UWP アプリで使用できます。 |

### <a name="item-templates-for-winui-3"></a>WinUI 3 用の項目テンプレート

次の項目テンプレートは、WinUI プロジェクトで使用できます。 これらの WinUI プロジェクト テンプレートにアクセスするには、**ソリューション エクスプローラー**でプロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** を選択し、 **[新しい項目の追加]** ダイアログの **[WinUI]** をクリックします。

![WinUI の項目テンプレート](images/winui-items-csharp.png)

| テンプレート | Language | 説明 |
|----------|----------|-------------|
| 空白のページ (WinUI) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Page** クラスから派生する新しいページを定義する XAML ファイルとコード ファイルを追加します。 |
| 空のウィンドウ (デスクトップの WinUI) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Window** クラスから派生する新しいウィンドウを定義する XAML ファイルとコード ファイルを追加します。 |
| カスタム コントロール (WinUI) | C++ および C# | 既定スタイルを使用してテンプレート化されたコントロールを作成するためのコード ファイルを追加します。 テンプレート化されたコントロールは、WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Control** クラスから派生します。<p></p>この項目テンプレートの使用方法を示すチュートリアルについては、「[C++/WinRT を使用した UWP および WinUI 3 アプリ用のテンプレート化された XAML コントロール](xaml-templated-controls-cppwinrt-winui3.md)」を参照してください。 テンプレート化されたコントロールの詳細については、「[カスタム XAML コントロール](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)」を参照してください。 |
| リソース ディクショナリ (WinUI) | C++ および C# | 空の、XAML リソースのキー付きコレクションを追加します。 詳細については、「[ResourceDictionary と XAML リソースの参照](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)」を参照してください。 |
| リソース ファイル (WinUI) | C++ および C# | アプリ用の文字列および条件付きリソースを格納するファイルを追加します。 この項目を使用して、アプリをローカライズすることができます。 詳細については、「[UI とアプリ パッケージ マニフェスト内の文字列をローカライズする](/windows/uwp/app-resources/localize-strings-ui-manifest)」を参照してください。 |
| ユーザー コントロール (WinUI) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Controls.UserControl** クラスから派生するユーザー コントロールを作成するための XAML ファイルとコード ファイルを追加します。 通常、ユーザー コントロールによって関連する既存のコントロールがカプセル化され、独自のロジックが提供されます。<p></p>ユーザー コントロールの詳細については、「[カスタム XAML コントロール](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)」を参照してください。 |

## <a name="bug-fixes-and-other-improvements-in-winui-3-preview-2"></a>WinUI 3 Preview 2 のバグ修正とその他の機能強化

Preview 2 のバグ修正とその他の更新の包括的な一覧を次に示します。 このリリースで解決された重大度の高いバグ修正の一覧については、[リリースのお知らせ](https://aka.ms/winui3/preview2-announcement)を参照してください。

> [!NOTE]
> WinUI 3 プレビュー 2 は WinUI 2 ライブラリのバージョン 2.4.2 を使用します。 

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged?view=net-5.0) と [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged?view=net-5.0) が C# デスクトップ アプリで正常に機能するようになりました
  - これにより、バックエンドで更新されている間、UI の更新されないコレクション コントロールに関連する他のいくつかの問題が解消されました。
  - *@hshristov さん、GitHub で[似た問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2490)を提出していただきありがとうございます。*
- Preview 2 はデスクトップ アプリの [.NET 5 Preview 5](/dotnet/api/?view=net-5.0) と互換性があるようになりました
- WinUI 3 は [WinUI 2.4](../winui2/release-notes/winui-2.4.md) と同等になりました。これには、[階層型の NavigationView](../winui2/release-notes/winui-2.4.md#hierarchical-navigation)、[ProgressRing](../winui2/release-notes/winui-2.4.md#progressring) などの新しいコントロールと機能が含まれています。
- クラッシュの修正:タッチでの [TabView](/windows/uwp/design/controls-and-patterns/tab-view) の使用
- [XAML コントロール ギャラリー サンプル](#xaml-controls-gallery-winui-3-preview-2-branch)の [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) には、左コンパクト モードではなく左モードが使用されるようになりました
- クラッシュの修正: 入力検証と [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) での高速な入力
  - *@paulovilla さん、GitHub で[この問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2563)を提出していただきありがとうございます。*
- クラッシュの修正: [TextBox](/windows/uwp/design/controls-and-patterns/text-box) メニューが表示されているときの XAML UI の操作
- 複数のページに移動した後、[XAML コントロール ギャラリー サンプル](#xaml-controls-gallery-winui-3-preview-2-branch)のタイトル テキストがスクランブルされなくなりました
- [WebView2](/microsoft-edge/webview2/) でタッチを使用しても、位置にわずかなオフセットが生じなくなりました
- WinUIEdit.dll のクラスは、Windows.UI.Text 名前空間から Microsoft.UI.Text 名前空間に移動されました
- クラッシュの修正: 複数選択モードでの [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) の項目の選択 (Windows 10 バージョン 1803)
- Point、Rect、および Size の各メンバーは、デスクトップ アプリの API の C# プロジェクションで Double 型になりました。
  - *@dotMorten さん、GitHub で[この問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2474)を提出していただきありがとうございます。*
- クラッシュの修正: .rtf ファイルでの [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) の使用
- [TabView](/windows/uwp/design/controls-and-patterns/tab-view) の閉じるボタンに空白のツールヒントが表示されなくなりました
- [イメージ](/windows/uwp/design/controls-and-patterns/images-imagebrushes) コントロールで SVG ファイルが正しくレンダリングされるようになりました
  - *@mqudsi さん、GitHub で[この問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2565)を提出していただきありがとうございます。*
- クラッシュの修正: Page 要素の使用または操作
- [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) の項目をタッチで選択すると、他のすべての項目の選択が解除されるようになりました (単一選択モード)。
- クラッシュの修正:特定のサイズの[スライダー](/windows/uwp/design/controls-and-patterns/slider) コントロールに設定された値による LayoutSliderException が発生しなくなりました 
  - *@hig-dev さん、GitHub で[この問題](https://github.com/microsoft/microsoft-ui-xaml/issues/477)を提出していただきありがとうございます。*
- クラッシュの修正: [ColorPicker](/windows/uwp/design/controls-and-patterns/color-picker) の使用によるシャットダウン時のクラッシュの発生
- クラッシュの修正: [Pivot](/windows/uwp/design/controls-and-patterns/pivot) の使用によるシャットダウン時のクラッシュの発生
- クラッシュの修正:Windows 10 バージョン 1803 でリソースが見つからないために発生する [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) のクラッシュ
- クラッシュの修正:[RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) カスタム エディターでのフォーカス 
- クラッシュの修正:[SemanticZoom](/windows/uwp/design/controls-and-patterns/semantic-zoom) 
- Mode=OneWay が暗黙的であるマークアップで、バインドが正常に機能するようになりました
  - *@tomasfabian さん、GitHub で[この問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2630)を提出していただきありがとうございます。*
- アニメーションの修正:[XAML コントロール ギャラリーのサンプル](#xaml-controls-gallery-winui-3-preview-2-branch)の新機能

## <a name="new-features-and-capabilities-introduced-in-winui-3-preview-1"></a>WinUI 3 Preview 1 で導入された新機能

次の機能は、WinUI 3 Preview 1 で導入されたものであり、WinUI 3 Preview 2 で引き続きサポートされます。

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
- オープン ソースの移行に必要な機能強化

WinUI 3 および WinUI のロードマップと利点の詳細については、GitHub の「[Windows UI Library Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)」を参照してください。

### <a name="provide-feedback-and-suggestions"></a>フィードバックやご提案の送信

[WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)にフィードバックをお寄せください。

## <a name="preview-2-limitations-and-known-issues"></a>Preview 2 の制限事項と既知の問題

Preview 2 リリースはプレビュー版にすぎません。 デスクトップ Win32 アプリに関するシナリオは特に新しいものです。 バグ、制限事項、およびその他の問題があることを想定してください。

次の項目は、WinUI 3 Preview 2 に関する既知の問題の一部です。 以下に列挙していない問題が見つかった場合は、[WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)で、既存の問題に投稿するか、または新規の問題として投稿して、Microsoft にお知らせください。

### <a name="platform-and-os-support"></a>プラットフォームと OS のサポート

WinUI 3 Preview 2 は、Windows 10 April 2018 Update (バージョン 1803 - ビルド 17134) 以降を実行する PC と互換性があります。

### <a name="developer-tools"></a>開発者ツール

- 現時点では、C# および C++/WinRT アプリのみがサポートされています
- デスクトップ アプリは .NET 5 および C# 8 をサポートしており、パッケージ化する必要があります
- UWP アプリは .NET Native および C# 7.3 をサポートしています
- Intellisense が不完全です
- ビジュアルなデザイナーがありません
- ホット リロードに対応していません
- ライブ ビジュアル ツリーには対応していません
- VS Code を使用した開発は未対応です
- 新しい C++ /cx アプリはサポートされていませんが、既存のアプリは引き続き機能します ( C++ /WinRT にできるだけ早く移行してください)
- WinUI 3 コンテンツは、プロセスごとに 1 つのウィンドウ、またはアプリごとに 1 つの ApplicationView でしか使用できません
- パッケージ化されていないデスクトップ展開はサポートしていません
- ARM64 はサポートしていません
- UWP アプリの C# カスタム コントロール: `Themes/Generic.xaml` は自動的に生成されません。 これを回避するには、クラスに手動で Themes フォルダーを作成し、その内部に `Generic.xaml` という XAML ファイルを配置します。
- WinUI カスタム コントロールをプロジェクトに追加した後、ファイルに "CustomControl.h" ヘッダーがない場合があります。 これを回避するには、ヘッダーを手動で `pch.h` ファイルに追加します。
- DataGrid、その他の Windows Community Toolkit コントロール、およびサード パーティのライブラリ コントロールを追加すると、ビルドが失敗することがあります。 この問題を回避するには、この統合された辞書を `App.xaml` ファイルに追加します。
  ```xaml
  <ResourceDictionary Source="ms-appx:///<library_name>/Themes/Generic.xaml"/>
  ```

### <a name="missing-platform-features"></a>ラットフォーム機能が不足しています

- Xbox のサポート
- HoloLens のサポート
- ウィンドウありのポップアップ
- 手描き入力のサポート
- 背景アクリル
- MediaElement および MediaPlayerElement
- RenderTargetBitmap
- MapControl
- SwapChainPanel は透明度をサポートしていません
- グローバルな表示でフォールバック動作 (純色ブラシ) を使用します
- 今回のリリースでは XAML Islands はサポートしていません
- サードパーティ製エコシステム ライブラリは完全には機能しません
- IME は機能しません

### <a name="known-issues"></a>既知の問題


- C# UWP アプリ:

  WinUI 3 フレームワークは WinRT コンポーネントのセットであり、WinRT には .NET と同様の型とオブジェクトがありますが、本質的に互換性がありません。  現在、C#/WinRT プロジェクションは .NET 5 における .NET と WinRT の相互運用を処理し、.NET 5 アプリで .NET インターフェイスを自由に使用できるようにします。 
  
  ただし、C#/WinRT は.NET Native アプリ間の相互運用を処理できないため、WinUI 3 API は UWP アプリに直接投影されます。 そのため、それらの同じ .NET インターフェイスは使用できなくなります。 **UWP アプリで .NET Native が使用されなくなると、この制限は存在しなくなります**。

  たとえば、`INotifyPropertyChanged` API はデスクトップ アプリの WinUI3 の `System.ComponentModel` 名前空間に投影されますが、UWP アプリ (およびすべての C++ アプリ) の WinUI3 の `Microsoft.UI.Xaml.Data` 名前空間に表示されます。 
  
  この問題の適用対象:
    - `INotifyPropertyChanged` (および関連する型)
    - `INotifyCollectionChanged`
    - `ICommand`

> [!Note] 
> `INotifyPropertyChanged` と `INotifyCollectionChanged` が予期したとおりに動作していない場合、`ObservableCollection<T>` クラスも影響を受けます。 独自のバージョンの `ObservableCollection<T>` を実装する例については、[このサンプル](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)を参照してください。 


## <a name="xaml-controls-gallery-winui-3-preview-2-branch"></a>XAML コントロール ギャラリー (WinUI 3 Preview 2 ブランチ)

すべての WinUI 3 Preview 2 コントロールおよび機能を含むサンプル アプリについては、[XAML コントロール ギャラリーの WinUI 3 Preview 2 ブランチ](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)を参照してください。

![WinUI 3 Preview 2 XAML コントロール ギャラリー アプリ](images/WinUI3XamlControlsGalleryP2.png)<br/>
*WinUI 3 Preview 2 XAML コントロール ギャラリー アプリの例*

サンプルをダウンロードするには、次のコマンドを使用して **winui3preview** ブランチを複製します。

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製した後、確実に、ローカルの Git 環境で **winui3preview** ブランチに切り替えてください。

```
git checkout winui3preview
```