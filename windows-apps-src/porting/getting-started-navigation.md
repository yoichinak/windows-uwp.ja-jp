---
title: ナビゲーションの概要
description: ユニバーサル Windows プラットフォーム (UWP) フレームクラスを使用して、複数のビューを含む Windows 10 アプリにページナビゲーションを追加する方法について説明します。
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ec5073664c911133b769ed9079a597459381a670
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174866"
---
# <a name="getting-started-navigation"></a>概要: ［ナビゲーション］


## <a name="adding-navigation"></a>ナビゲーションの追加

iOS では、アプリのナビゲーション用に **UINavigationController** クラスが用意されています。ビューのプッシュ/ポップ操作を通じて、アプリを定義する **UIViewControllers** の階層を作ることができます。

これに対して、複数のビューを含む Windows 10 アプリはナビゲーションに対してさらに多くの Web サイト アプローチを行います。 ユーザーがコントロールをクリックしてページ間を移動し、アプリ内を進むことを考えてみることができます。 詳しくは、「[ナビゲーション デザインの基本](../design/basics/navigation-basics.md)」をご覧ください。

Windows 10 アプリでこのナビゲーションを管理する方法の1つは、 [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) クラスを使用することです。 以下のチュートリアルでは実際に試す方法を示しています。

以前に開始したソリューションに戻り、**MainPage.xaml** ファイルを開いて、**[デザイン]** ビューにボタンを追加します。 このボタンの **Content** プロパティを "Button" から "Go To Page" に変更します。 次に、ボタンの **Click** イベントのハンドラーを、次の図に示すように作成します。 作成方法がわからない場合は、前のセクションのチュートリアルを見直してください (ヒント: **[デザイン]** ビューにあるボタンをダブルクリックします)。

![Visual Studio でのボタンとそのクリック イベントの追加](images/ios-to-uwp/vs-go-to-page.png)

新しいページを追加しましょう。 **[ソリューション]** ビューで、**[プロジェクト]** メニュー、**[新しい項目の追加]** の順にタップします。 次の図に示すように、**[空白のページ]** をタップし、**[追加]** をタップします。

![Visual Studio での新しいページの追加](images/ios-to-uwp/vs-add-new-page.png)

次に、BlankPage.xaml ファイルにボタンを追加します。 ここでは、AppBarButton コントロールを使い、ボタンに "前に戻る矢印" の画像を設定します。**[XAML]** ビューで、` <AppBarButton Icon="Back"/>` を `<Grid> </Grid>` 要素の間に追加します。

次に、イベントハンドラーをボタンに追加してみましょう。 **デザイン** ビューでコントロールをダブルクリックし、次の図に示すように、Microsoft Visual Studio "AppBarButton click" というテキストを \_ **クリック** ボックスに追加して、対応するイベントハンドラーを BlankPage.xaml.cs ファイルに追加して表示します。

![Visual Studio での戻るボタンとそのクリック イベントの追加](images/ios-to-uwp/vs-add-back-button.png)

BlankPage.xaml ファイルの **XAML** ビューに戻り、`<AppBarButton>` 要素の Extensible Application Markup Language (XAML) コードが次のようになっていることを確かめます。

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

BlankPage.xaml.cs ファイルに戻り、以下のコードを追加して、ユーザーがボタンをタップすると前のページに戻るようにします。

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

最後に、MainPage.xaml.cs ファイルを開き、次のコードを追加します。 このコードは、ユーザーがボタンをタップしたときに BlankPage を開くためのコードです。

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

それでは、プログラムを実行してみましょう。 [Go To Page] ボタンをタップすると、他のページに進みます。矢印スタイルの戻るボタンをタップすると、前のページに戻ります。

ページのナビゲーションは、[**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) クラスによって管理されます。 IOS の **UINavigationController** クラスで **Pushviewcontroller** メソッドと **popviewcontroller** メソッドが使用されているため、UWP アプリの **Frame** クラスは [**Navigate**](/uwp/api/windows.ui.xaml.controls.frame.navigate) メソッドと [**GoBack**](/uwp/api/windows.ui.xaml.controls.frame.goback) メソッドを提供します。 **Frame** クラスには、名前から推測されるとおりに動作する [**GoForward**](/uwp/api/windows.ui.xaml.controls.frame.goforward) というメソッドもあります。

このチュートリアルでは、ナビゲーションを行うたびに BlankPage の新しいインスタンスが作成されます。 (前のインスタンスは自動的に*解放*されます)。 毎回新しいインスタンスが作成されることがないようにするには、BlankPage.xaml.cs ファイル内の BlankPage クラスのコンストラクターに以下のコードを追加します。 これにより、[**NavigationCacheMode**](/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) 動作が有効になります。

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

また、**Frame** クラスの [**CacheSize**](/uwp/api/windows.ui.xaml.controls.frame.cachesize) プロパティを取得または設定すると、キャッシュするナビゲーションの履歴のページ数を管理できます。

ナビゲーションについて詳しくは、「[ナビゲーション](../design/basics/navigation-basics.md)」と「[XAML パーソナリティ アニメーションのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20personality%20animations%20sample%20(Windows%208))」をご覧ください。

**メモ**   JavaScript と HTML を使用した UWP アプリのナビゲーションの詳細については、「[クイックスタート: 単一ページナビゲーションの使用](/previous-versions/windows/apps/hh452768(v=win.10))」を参照してください。
 
### <a name="next-step"></a>次のステップ

[概要: アニメーション](getting-started-animation.md)