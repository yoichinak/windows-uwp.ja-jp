---
description: The practice of defining UI in the form of declarative XAML markup translates extremely well from Windows Phone Silverlight to Universal Windows Platform (UWP) apps.
title: Porting Windows Phone Silverlight XAML and UI to UWP
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eeb8cb8a8b71123c3a5a94eea316621e5f93fe8e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259077"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>Porting Windows Phone Silverlight XAML and UI to UWP



前のトピックは、「[トラブルシューティング](wpsl-to-uwp-troubleshooting.md)」でした。

The practice of defining UI in the form of declarative XAML markup translates extremely well from Windows Phone Silverlight to Universal Windows Platform (UWP) apps. システムのリソース キーの参照の更新、いくつかの要素型名の変更、"clr-namespace" から "using" への変更を行うことによって、大きなマークアップ セクションで互換性が得られることがわかります。 プレゼンテーション層のビュー モデルの命令型コード、および UI 要素を操作するコードの大半でも、簡単に移植できます。

## <a name="a-first-look-at-the-xaml-markup"></a>初めての XAML マークアップ

The previous topic showed you how to copy your XAML and code-behind files into your new Windows 10 Visual Studio project. Visual Studio XAML デザイナーで強調表示され、認識される最初の問題の 1 つは、XAML ファイルのルートで `PhoneApplicationPage` 要素がユニバーサル Windows プラットフォーム (UWP) プロジェクトに対して有効ではないことです。 In the previous topic, you saved a copy of the XAML files that Visual Studio generated when it created the Windows 10 project. MainPage.xaml の該当バージョンを開くと、ルートに [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) 名前空間の [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 型があることに気付きます。 したがって、すべての `<phone:PhoneApplicationPage>` 要素を `<Page>` に変更できます (プロパティ要素の構文を忘れないでください)。また、`xmlns:phone` 宣言は削除できます。

For a more general approach to finding the UWP type that corresponds to a Windows Phone Silverlight type, you can refer to [Namespace and class mappings](wpsl-to-uwp-namespace-and-class-mappings.md).

## <a name="xaml-namespace-prefix-declarations"></a>XAML 名前空間のプレフィックス宣言


ビュー、おそらくビュー モデル インスタンスまたは値コンバーターでカスタム型のインスタンスを使う場合、XAML マークアップに XAML 名前空間のプレフィックス宣言が含まれます。 The syntax of these differs between Windows Phone Silverlight and the UWP. 例をいくつか紹介します。

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

"clr-namespace" を "using" に変更し、アセンブリ トークンとセミコロンを削除します (アセンブリが推論されます)。 結果は次のようになります。

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

システムによって種類が定義されるリソースが存在する場合があります。

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

UWP で、"System" プレフィックス宣言を省略し、(既に宣言されている) "x" プレフィックスを代わりに使います。

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>命令型コード


ビュー モデルは、UI の種類を参照する命令型コードが存在する場所の 1 つです。 もう 1 つの場所は、UI 要素を直接操作するコード ビハインド ファイルです。 たとえば、次のようなコード行がまだコンパイルされていないことに気が付くことがあります。


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage** is in the **System.Windows.Media.Imaging** namespace in Windows Phone Silverlight, and a using directive in the same file allows **BitmapImage** to be used without namespace qualification as in the snippet above. 同様の場合に、Visual Studio で型名 (**BitmapImage**) を右クリックし、コンテキスト メニューの **[解決]** コマンドを使って新しい名前空間のディレクティブをファイルに追加できます。 この場合、該当する型が UWP に存在している、[**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) 名前空間が追加されます。 **System.Windows.Media.Imaging** の using ディレクティブは削除できます。また、前記のスニペットのようなコードの移植で必要になるのはこれだけです。 When you're done, you'll have removed all Windows Phone Silverlight namespaces.

古い名前空間の型を新しい名前空間の同じ型でマッピングするこのような簡単なケースでは、ソース コードに一括変更を加えるために、Visual Studio の **[検索と置換]** コマンドを使うこともできます。 **[解決]** コマンドは、型の新しい名前空間を検索するための効果的な方法です。 別の例として、すべての "System.Windows" を "Windows.UI.Xaml" に置換できます。 これによって基本的には、該当する名前空間を参照するすべての using ディレクティブおよび完全修飾されたすべての型名が移植されます。

古い using ディレクティブをすべて削除し、新しい using ディレクティブを追加したら、Visual Studio の **[using の整理]** コマンドを使ってディレクティブを並べ替えて、未使用のディレクティブを削除できます。

命令型コードの修正がパラメーターの型の変更のみになることもあります。 Other times, you will need to use UWP APIs instead of .NET APIs for Windows Runtime 8.x apps. To identify which APIs are supported, use the rest of this porting guide in combination with [.NET for Windows Runtime 8.x apps overview](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)) and the [Windows Runtime reference](https://docs.microsoft.com/uwp/api/).

また、プロジェクトのビルド段階にただ進むだけであれば、重要でないコードをコメントアウトするか、スタブを挿入できます。 次に、このセクションの以降のトピック (および前のトピック「[トラブルシューティング](wpsl-to-uwp-troubleshooting.md)」) を参考にして、ビルドとランタイムの問題が解決して移植が完了するまで一度に 1 つの問題について反復作業を行います。

## <a name="adaptiveresponsive-ui"></a>アダプティブ/応答性の高い UI

Because your Windows 10 app can run on a potentially wide range of devices—each with its own screen size and resolution—you'll want to go beyond the minimal steps to port your app and you'll want to tailor your UI to look its best on those devices. アダプティブな Visual State Manager の機能を使って、ウィンドウのサイズを動的に検出し、それに応じてレイアウトを変更できます。その方法を示す例を、Bookstore2 ケース スタディの「[アダプティブ UI](wpsl-to-uwp-case-study-bookstore2.md)」に示します。

## <a name="alarms-and-reminders"></a>アラームとリマインダー

**Alarm** クラスまたは **Reminder** クラスを使うコードは、バックグラウンド タスクを作成、登録して、関連する時間にトーストを表示するために、[**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) クラスを使って移植します。 「[バックグラウンド処理](wpsl-to-uwp-business-and-data.md)」と「[トースト](#toasts)」をご覧ください。

## <a name="animation"></a>アニメーション

キーフレーム アニメーションと from/to アニメーションに対して推奨される代替機能として、UWP アプリで UWP アニメーション ライブラリが利用できるようになりました。 こうしたアニメーションはスムーズかつ適切に表示されるように設計、調整されているために、組み込みのアプリのように Windows に統合されてアプリが動作します。 「[クイック スタート: ライブラリのアニメーションを使った UI のアニメーション化](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))」をご覧ください。

UWP アプリでキーフレーム アニメーションまたは from/to アニメーションを使う場合、新しいプラットフォームで導入された独立型アニメーションと依存型アニメーションの相違について理解することをお勧めします。 「[アニメーションとメディアの最適化](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-animations-and-media)」をご覧ください。 UI スレッドで実行するアニメーション (たとえば、レイアウト プロパティをアニメーションするもの) は、依存型アニメーションと呼ばれ、新しいプラットフォーム上での実行時に、次の 2 つのいずれかを行わなければ無効になります。 いずれも、異なるプロパティ ([**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) など) をアニメーションするためにターゲットを変更できるので、独立型にできます。 アニメーション要素で `EnableDependentAnimation="True"` を設定することによって、スムーズな実行を保証できないアニメーションを実行する意図を確認できます。 新しいアニメーションの作成のために Blend for Visual Studio を使っている場合、必要であればこのプロパティが自動的に設定されます。

## <a name="back-button-handling"></a>"戻る" ボタンの処理

In a Windows 10 app, you can use a single approach to handling the back button and it will work on all devices. モバイル デバイスでは、このボタンはデバイス上の静電容量式のボタンまたはシェル内のボタンとして提供されます。 デスクトップ デバイスでは、アプリ内で戻るナビゲーションが可能な場合には常にアプリのクロムにボタンを追加します。このボタンは、ウィンドウ表示されたアプリのタイトル バーまたはタブレット モードのタスク バーに表示されます。 "戻る" ボタンのイベントはすべてのデバイス ファミリに共通するユニバーサルな概念であり、ハードウェアまたはソフトウェアに実装されるボタンは同じ [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) イベントを発生させます。

次の例は、すべてのデバイス ファミリについて機能し、同じ処理をすべてのページに適用し、ナビゲーションを確認する必要がない場合 (未保存の変更に関する警告を表示するなど) に適しています。

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide a back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

プログラムを使ったアプリの終了に関しても、すべてのデバイス ファミリに対する単一のアプローチがあります。

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="binding-and-compiled-bindings-with-xbind"></a>バインドおよび {x:Bind} でコンパイル済みのバインド

次に、バインドに関するトピックをいくつか示します。

-   UI 要素から "データ" (つまり、ビュー モデルのプロパティとコマンド) へのバインド
-   UI 要素から別の UI 要素へのバインド
-   監視可能なビュー モデルの記述 (つまり、プロパティ値の変更時およびコマンドの可用性の変更時に通知が発生します)

こうした要素はすべて、引き続きサポートされていますが、名前空間には違いがあります。 たとえば、**System.Windows.Data.Binding** は [**Windows.UI.Xaml.Data.Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) にマップし、**System.ComponentModel.INotifyPropertyChanged** は [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) にマップして、**System.Collections.Specialized.INotifyPropertyChanged** は [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.INotifyCollectionChanged) にマップします。

Windows Phone Silverlight app bars and app bar buttons can't be bound like they can in a UWP app. アプリ バーとそのボタンを構築し、プロパティとローカライズされた文字列にバインドして、イベントを処理する命令型コードを使う場合もあります。 その場合、プロパティとコマンドにバインドされた宣言型マークアップ、および静的なリソース参照によって置き換えることで、該当する命令型コードを移植し、アプリの安全性と保守性を段階的に高めることができます。 Visual Studio または Blend for Visual Studio を使って、他の XAML 要素と同様に、UWP アプリ バーのボタンのバインドとスタイル設定を行うことができます。 UWP アプリでは、使う型名は [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) および [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) であることに注意してください。

ただし、現在 UWP アプリのバインド関連の機能には以下の制限があります。

-   データ エントリ検証と [**IDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.idataerrorinfo) インターフェイスおよび [**INotifyDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifydataerrorinfo) インターフェイスには、サポートが組み込まれていません。
-   The [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) class does not include the extended formatting properties available in Windows Phone Silverlight. ただし、[**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) を実装してカスタム書式設定を提供することはできます。
-   [  **IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) メソッドは、言語文字列を、[**CultureInfo**](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) オブジェクトではなくパラメーターとして受け取ります。
-   [  **CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) クラスには、並べ替えとフィルター処理、さまざまなグループへの作業のグループ化のサポートが組み込まれていません。 詳しくは、「[データ バインディングの詳細](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)」と[データ バインディングのサンプルに関するページ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)をご覧ください。

Although the same binding features are still largely supported, Windows 10 offers the option of a new and more performant binding mechanism called compiled bindings, which use the {x:Bind} markup extension. [データ バインディング: XAML データ バインディングの新しい拡張機能によるアプリのパフォーマンスの向上に関するページ](https://channel9.msdn.com/Events/Build/2015/3-635)と [x:Bind のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)をご覧ください。

## <a name="binding-an-image-to-a-view-model"></a>ビュー モデルへの画像のバインド

[  **Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) プロパティは、ビュー モデルの [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource) 型であるどのプロパティにもバインドできます。 Here's a typical implementation of such a property in a Windows Phone Silverlight app:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

UWP アプリでは、ms-appx [URI スキーム](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10))を使います。 残るコードを変更せずに維持するために、**System.Uri** コンストラクターの異なるオーバーロードを使って、ベース URI に ms-appx URI スキームを格納し、パスの残る部分を追加できます。 次に例を示します。

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

これによって、ビュー モデル、画像のパス プロパティのパス値、XAML マークアップのバインドの残る部分をすべて、まったく変更せずに維持できます。

## <a name="controls-and-control-stylestemplates"></a>コントロールとコントロール スタイル/テンプレート

Windows Phone Silverlight apps use controls defined in the **Microsoft.Phone.Controls** namespace and the **System.Windows.Controls** namespace. XAML UWP アプリは、[**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) 名前空間で定義されたコントロールを使います。 The architecture and design of XAML controls in the UWP is virtually the same as Windows Phone Silverlight controls. ただし、使用可能なコントロール セットの向上と Windows アプリとの一体化のために、若干の変更が加えられています。 具体的な例をいくつか紹介します。

| コントロール名 | [Change] |
|--------------|--------|
| ApplicationBar | [Page.TopAppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.topappbar) プロパティです。 |
| ApplicationBarIconButton | UWP の相当要素は [Glyph](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon.glyph) プロパティです。 PrimaryCommands は CommandBar のコンテンツ プロパティです。 XAML パーサーは、コンテンツ プロパティの値として要素の内部 xml を解釈します。 |
| ApplicationBarMenuItem | UWP の相当要素は、メニュー項目のテキストに設定された [AppBarButton.Label](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.label) です。 |
| ContextMenu (Windows Phone Toolkit 内) | 単一選択ポップアップの場合は、[Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout) を使います。 |
| ControlTiltEffect.TiltEffect クラス | UWP アニメーション ライブラリのアニメーションは、共通のコントロールの既定のスタイルに組み込まれています。 「[ポインター操作のアニメーション化](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))」をご覧ください。 |
| グループ化されたデータを含む LongListSelector | The Windows Phone Silverlight LongListSelector functions in two ways, which can be used in concert. まず、キーによってグループ化したデータを表示できます、たとえば、名前の一覧を最初の文字によってグループ化できます。 次に、2 つのセマンティック ビュー (名前などの項目のグループ化されたリストと、最初の文字などのグループ キー自体に限られるリスト) の間で "ズーム" できます。 UWP では、[リスト ビュー コントロールとグリッド ビュー コントロールのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists)に従ってグループ化されたデータを表示できます。 |
| フラット データを含む LongListSelector | For performance reasons, in the case of very long lists, we recommended LongListSelector instead of a Windows Phone Silverlight list box even for flat, non-grouped data. UWP アプリでは、データのグループ化が可能であるかどうかにかかわらず、長い項目リストで [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) を使うことをお勧めします。 |
| Panorama | The Windows Phone Silverlight Panorama control maps to the [Guidelines for hub controls in Windows Runtime 8.x apps](https://docs.microsoft.com/windows/uwp/controls-and-patterns/hub) and Guidelines for the hub control. <br/> Panorama コントロールは、最後のセクションから最初のセクションに折り返され、背景画像はセクションを基準とした視差効果で移動します。 [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) セクションは折り返されず、視差効果は使われません。 |
| Pivot | The UWP equivalent of the Windows Phone Silverlight Pivot control is [Windows.UI.Xaml.Controls.Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot). これはすべてのデバイス ファミリで利用できます。 |

**Note**   The PointerOver visual state is relevant in custom styles/templates in Windows 10 apps, but not in Windows Phone Silverlight apps. There are other reasons why your existing custom styles/templates may not be appropriate for Windows 10 apps, including system resource keys you are using, changes to the sets of visual states used, and performance improvements made to the Windows 10 default styles/templates. We recommend that you edit a fresh copy of a control's default template for Windows 10 and then re-apply your style and template customization to that.

UWP のコントロールについて詳しくは、「[機能別コントロール](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function)」、「[コントロールの一覧](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)」、「[コントロールのガイドライン](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index)」をご覧ください。

##  <a name="design-language-in-windows10"></a>Design language in Windows 10

There are some differences in design language between Windows Phone Silverlight apps and Windows 10 apps. 詳しくは、「[Design](https://developer.microsoft.com/en-us/windows/apps/design)」(UWP アプリの設計) をご覧ください。 デザイン言語に変更が加えられていますが、設計原則は維持されています。細部にまで注意を払いながら、簡潔さを追求しています。そのために、クロムよりもコンテンツを優先し、視覚要素を大幅に減らし、真のデジタル領域を常に意識しています。また、視覚的な階層の利用 (特に文字体裁に対して)、グリッド内でのデザイン、滑らかなアニメーションを使ったエクスペリエンスの実現も行っています。

## <a name="localization-and-globalization"></a>ローカリゼーションとグローバリゼーション

For localized strings, you can re-use the .resx file from your Windows Phone Silverlight project in your UWP app project. ファイルをコピーしてプロジェクトに追加し、検索メカニズムによって既定で検索されるように名前を Resources.resw に変更します **[ビルド アクション]** を **[PRIResource]** に設定し、 **[出力ディレクトリにコピー]** を **[コピーしない]** に設定します。 次に、XAML 要素で **x:Uid** 属性を指定することにより、マークアップで文字列を使うことができます。 「[クイック スタート: 文字列リソースの使用](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10))」をご覧ください。

Windows Phone Silverlight apps use the **CultureInfo** class to help globalize an app. UWP アプリは、MRT (Modern Resource Technology) を使います。これによって、実行時および Visual Studio デザイン サーフェイスで、アプリ リソース (ローカライズ、サイズ調整、テーマ) の動的な読み込みが可能になります。 詳しくは、「[ファイル、データ、グローバリゼーションのガイドライン](https://docs.microsoft.com/windows/uwp/design/usability/index)」をご覧ください。

「[**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues)」トピックでは、デバイス ファミリのリソースを選ぶ要因に基づいてデバイス ファミリ固有のリソースを読み込む方法について説明しています。

## <a name="media-and-graphics"></a>メディアとグラフィックス

UWP のメディアとグラフィックスに関する説明では、Windows の設計原則で、視覚的な複雑さや混乱を含めて、不用なあらゆるものの極度の簡略化を促している点に注意してください。 Windows 設計は、明確かつ明瞭な視覚効果、文字体裁、モーションによって特徴付けられます。 アプリが同じ原則に従えば、組み込みアプリのように振る舞うことができます。

Windows Phone Silverlight has a **RadialGradientBrush** type which is not present in the UWP, although other [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) types are. 場合によっては、ビットマップでも同様の効果を得ることができます。 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) および XAML C++ UWP では、Direct2D により[放射状グラデーションのブラシを作成](https://docs.microsoft.com/windows/desktop/Direct2D/how-to-create-a-radial-gradient-brush)できることに注意してください。

Windows Phone Silverlight has the **System.Windows.UIElement.OpacityMask** property, but that property is not a member of the UWP [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) type. 場合によっては、ビットマップでも同様の効果を得ることができます。 また、[Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) および XAML C++ UWP アプリでは、Direct2D により[不透明マスクを作成](https://docs.microsoft.com/windows/desktop/Direct2D/opacity-masks-overview)できます。 ただし、**OpacityMask** の一般的な使用事例の 1 つに、淡色テーマと濃色テーマの両方に適合する単一のビットマップを使うことがあります。 ベクター グラフィック (次に示す円グラフなど) では、テーマに対応するシステム ブラシを使うことができます。 ただし、テーマに対応するビットマップ (次に示すチェック マークなど) を作成するには、別のアプローチが必要です。

![テーマに対応するビットマップ](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

In a Windows Phone Silverlight app, the technique is to use an alpha mask (in the form of a bitmap) as the **OpacityMask** for a **Rectangle** filled with the foreground brush:

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

UWP アプリにこれを移植する最も簡単な方法は、[**BitmapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon) を使うことです。

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

Here, winrt\_check.png is an alpha mask in the form of a bitmap just as wpsl\_check.png is, and it could very well be the same file. However, you may want to provide several different sizes of winrt\_check.png to be used for different scaling factors. この詳細および **Width** 値と **Height** 値の変更については、このトピックの「[表示ピクセルと有効ピクセル、視聴距離、スケール ファクター](#view-or-effective-pixels-viewing-distance-and-scale-factors)」をご覧ください。

淡色テーマと濃色テーマのビットマップ形式に違いがある場合に適切であるより一般的なアプローチは、2 つの画像アセット (一方が淡色テーマ用の濃色の前景、他方が濃色テーマ用の淡色の前景) を使うことです。 For further details about how to name this set of bitmap assets, see [Tailor your resources for language, scale, and other qualifiers](../app-resources/tailor-resources-lang-scale-contrast.md). イメージ ファイル セットに正しい名前を付けた後、次のようにしてルート名を使って抽象内で参照できます。

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

In Windows Phone Silverlight, the **UIElement.Clip** property can be any shape that you can express with a **Geometry** and is typically serialized in XAML markup in the **StreamGeometry** mini-language. UWP では、[**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) プロパティの型は [**RectangleGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry) です。したがって、四角形の領域のみを切り取ることができます。 ミニ言語を使った四角形の定義を許可することは寛容すぎる場合もあります。 したがって、マークアップ内の切り取り領域を移植するには、**Clip** 属性構文を置き換え、次のようなプロパティ要素構文にします。

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

[Microsoft DirectX](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-layers-overview) アプリおよび XAML C++ UWP アプリで Direct2D により[レイヤー内でマスクとして任意のジオメトリを使用](https://docs.microsoft.com/windows/desktop/directx)できることに注意してください。

## <a name="navigation"></a>ナビゲーション

When you navigate to a page in a Windows Phone Silverlight app, you use a Uniform Resource Identifier (URI) addressing scheme:

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

UWP アプリでは、[**Frame.Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) メソッドを呼び出し、(ページの XAML マークアップ定義の **x:Class** 属性によって定義された) 移動先ページの種類を指定します。


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

You define the startup page for a Windows Phone Silverlight app in WMAppManifest.xml:

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

UWP アプリでは、起動ページを定義するために命令型コードを使います。 次の例は、そのための方法を示す App.xaml.cs からのコードです。

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

URI マッピングとフラグメント ナビゲーションは URI ナビゲーションの手法であり、したがって URI に基づく UWP ナビゲーションには該当しません。 URI マッピングは、ページが異なるフォルダー、したがって異なる相対パスに移動したときに脆弱性と保守性の問題が生じる URI 文字列によるターゲット ページの特定における弱い型の特性への対応として用意されているものです。 UWP アプリでは、厳密に型指定され、コンパイラで確認される型ベースのナビゲーションを使っており、URI マッピングの解決における問題がありません。 フラグメント ナビゲーションの使用事例は、ページでコンテンツの特定のフラグメントにビューをスクロールするか、または表示できるように、ターゲット ページにコンテキストを渡すことです。 [  **Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) メソッドの呼び出し時にナビゲーション パラメーターを渡すことでも、これは実現できます。

詳しくは、「[ナビゲーション](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)」をご覧ください。

## <a name="resource-key-reference"></a>リソースのキーの参照

The design language has evolved for Windows 10 and consequently certain system styles have changed, and many system resource keys have been removed or renamed. Visual Studio の XAML マークアップ エディターでは、解決できないリソース キーへの参照が強調表示されます。 たとえば XAML マークアップ エディターでは、スタイル キー `PhoneTextNormalStyle` への参照の下に赤い波線が引かれます。 これを修正しない場合、エミュレーターかデバイスに展開しようとしたときにアプリが直ちに終了します。 したがって、XAML マークアップの正確性に関する作業に着手することが重要です。 また、そのような問題を検出するために Visual Studio が優れたツールであることがわかります。

この後の「[テキスト](#text)」もご覧ください。

## <a name="status-bar-system-tray"></a>ステータス バー (システム トレイ)

システム トレイ (`shell:SystemTray.IsVisible` により XAML マークアップで設定される) は、現在ではステータス バーと呼ばれており、既定で示されます。 [  **Windows.UI.ViewManagement.StatusBar.ShowAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.showasync) メソッドと [**HideAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) メソッドを呼び出すことによって、表示するかどうかを命令型コードで制御できます。

## <a name="text"></a>テキスト

テキスト (または文字体裁) は UWP アプリの重要な要素です。移植するときには、ビューの視覚的なデザインが新しいデザイン言語に適合するように、ビューの視覚的なデザインを再検討することが必要になる場合があります。 次の図を使って、利用可能な UWP の **TextBlock** システム スタイルを見つけます。 Find the ones that correspond to the Windows Phone Silverlight styles you used. Alternatively, you can create your own universal styles and copy the properties from the Windows Phone Silverlight system styles into those.

![Windows 10 アプリのシステム TextBlock スタイル](images/label-uwp10stylegallery.png)

System TextBlock styles for Windows 10 apps

In a Windows Phone Silverlight app, the default font family is Segoe WP. In a Windows 10 app, the default font family is Segoe UI. この結果、アプリでのフォント メトリックの表示が異なる可能性があります。 If you want to reproduce the look of your Windows Phone Silverlight text, you can set your own metrics using properties such as [**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) and [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy). 詳しくは、「[フォントのガイドライン](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)」と「[UWP アプリの設計](https://developer.microsoft.com/en-us/windows/apps/design)」をご覧ください。

## <a name="theme-changes"></a>テーマの変更

For a Windows Phone Silverlight app, the default theme is dark by default. For Windows 10 devices, the default theme has changed, but you can control the theme used by declaring a requested theme in App.xaml. たとえば、すべてのデバイスで濃色テーマを使うには、`RequestedTheme="Dark"` をルートの Application 要素に追加します。

## <a name="tiles"></a>タイル

Tiles for UWP apps have behaviors similar to Live Tiles for Windows Phone Silverlight apps, although there are some differences. たとえば、セカンダリ タイルを作成するために **Microsoft.Phone.Shell.ShellTile.Create** メソッドを呼び出すコードは、[**SecondaryTile.RequestCreateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestcreateasync) を呼び出すように移植する必要があります。 Here is a before-and-after example, first the Windows Phone Silverlight version:


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

次に相当する UWP の要素を示します。

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

**Microsoft.Phone.Shell.ShellTile.Update** メソッドまたは **Microsoft.Phone.Shell.ShellTileSchedule** クラスによりタイルを更新するコードは、[**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager) クラス、[**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) クラス、[**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) クラス、[**ScheduledTileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledTileNotification) クラスを使うように移植する必要があります。

タイル、トースト、バッジ、バナー、通知について詳しくは、「[タイルの作成](https://docs.microsoft.com/previous-versions/windows/apps/hh868260(v=win.10))」と「[タイル、バッジ、トースト通知の操作](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))」をご覧ください。 UWP タイルに使うビジュアル アセットのサイズの仕様については、「[タイルとトーストのビジュアル資産](https://docs.microsoft.com/previous-versions/windows/apps/hh781198(v=win.10))」をご覧ください。

## <a name="toasts"></a>トースト

**Microsoft.Phone.Shell.ShellToast** クラスによりトーストを表示するコードは、[**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) クラス、[**ToastNotifier**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier) クラス、[**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) クラス、[**ScheduledToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) クラスを使うように移植する必要があります。 モバイル デバイスでは、"トースト" の利用者向け用語が "バナー" であることに注意してください。

「[タイル、バッジ、トースト通知の操作](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))」をご覧ください。

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>表示ピクセルと有効ピクセル、視聴距離、スケール ファクター

Windows Phone Silverlight apps and Windows 10 apps differ in the way they abstract the size and layout of UI elements away from the actual physical size and resolution of devices. A Windows Phone Silverlight app uses view pixels to do this. With Windows 10, the concept of view pixels has been refined into that of effective pixels. 有効ピクセルの用語の説明、有効ピクセルが何をするものなのか、および有効ピクセルで使うことができる追加の値について、以下に示します。

一般的な考えとは異なり、"解像度" という用語はピクセル密度の測定値を表しており、ピクセル数ではありません。 "有効解像度" は、画像またはグリフを構成する物理ピクセルを解決して、デバイスの視聴距離と物理ピクセル サイズでの目視による相違の度合を取得する方法です (物理ピクセル サイズの逆数であるピクセル密度)。 有効解像度は、ユーザー中心であるために、エクスペリエンスの構築に適したメトリックです。 すべての要因について理解し、UI 要素のサイズを制御することによって、ユーザーのエクスペリエンスを適切なものにすることができます。

To a Windows Phone Silverlight app, all phone screens are exactly 480 view pixels wide, without exception, no matter how many physical pixels the screen has, nor what its pixel density or physical size is. This means that an **Image** element with `Width="48"` will be exactly one tenth of the width of the screen of any phone that can run the Windows Phone Silverlight app.

To a Windows 10 app, it is *not* the case that all devices are some fixed number of effective pixels wide. これは、UWP アプリが広範なデバイスで実行できることから、おそらく明白です。 デバイスによって、有効ピクセルの幅の値が異なります。その範囲は、320 epx (最小のデバイス) から 1024 epx (一般的なサイズのモニター)、またはそれ以上のさらに広い幅になります。 これまでと同様に、自動的にサイズ調整される要素と動的レイアウト パネルを引き続き使うことで十分に対応できます。 ただし、場合によっては、UI 要素のプロパティを XAML マークアップで固定サイズに設定することがあります。 スケール ファクターは、アプリが実行されているデバイスやユーザーが行った表示設定に応じて、アプリに自動的に適用されます。 スケール ファクターによって、さまざまな画面サイズでユーザーに対してほぼ一定サイズのタッチ (または読み取り) ターゲットを提示するように、すべての UI 要素を固定サイズで維持できます。 また、動的レイアウトと共に使うことで、UI は単にさまざまなデバイスで光学的なスケーリングを行うだけでなく、利用可能な領域に合わせて適切な量のコンテンツを表示するために必要となる処理も実行します。

Because 480 was formerly the fixed width in view pixels for a phone-sized screen, and that value is now typically smaller in effective pixels, a rule of thumb is to multiply any dimension in your Windows Phone Silverlight app markup by a factor of 0.8.

すべてのディスプレイで最適なアプリのエクスペリエンスが実現できるように、一連のサイズで各ビットマップ アセットを作成し、各アセットが特定のスケール ファクターに適合するように設定することをお勧めします。 ただし、100% スケール、200% スケール、および 400% スケール (この優先順位で) でアセットを作成するほうが、多くの場合、すべての中間スケール ファクターで適切な結果を得ることができます。

**Note**  If, for whatever reason, you cannot create assets in more than one size, then create 100%-scale assets. Microsoft Visual Studio では、UWP アプリの既定のプロジェクト テンプレートには 1 つのサイズのみのブランド アセット (タイル イメージとロゴ) が用意されていますが、これらは 100% のスケールではありません。 独自のアプリのアセットを作成する場合は、このセクションに示したガイドラインに従って、100%、200%、400% のサイズを用意し、アセット パックを使います。

複雑なアートワークがある場合は、さらに多くのサイズに対応したアセットが必要になることがあります。 ベクター アートを使って作業を始める場合は、どのようなスケール ファクターでも高品質なアセットを比較的簡単に生成できます。

We don't recommend that you try to support all of the scale factors, but the full list of scale factors for Windows 10 apps is 100%, 125%, 150%, 200%, 250%, 300%, and 400%. すべてのスケール ファクターのアセットを提供した場合、ストアでは、各デバイスに合った適切なサイズのアセットが選ばれ、それらのアセットのみがダウンロードされます。 ストアでは、デバイスの DPI に基づいて、ダウンロードするアセットが選ばれます。

詳しくは、「[UWP アプリ用レスポンシブ デザイン 101](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)」をご覧ください。

## <a name="window-size"></a>ウィンドウ サイズ

UWP アプリでは、命令型コードを使って最小サイズ (幅と高さ) を指定できます。 既定の最小サイズは 500 x 320 epx で、このサイズは受け入れられる最も小さいサイズでもあります。 受け入れられる最も大きいサイズは 500 x 500 epx です。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

次のトピックは、「[入出力、デバイス、アプリ モデルの移植](wpsl-to-uwp-input-and-sensors.md)」です。

## <a name="related-topics"></a>関連トピック

* [Namespace and class mappings](wpsl-to-uwp-namespace-and-class-mappings.md)
