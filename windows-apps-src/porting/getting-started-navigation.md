---
title: ナビゲーションの概要
description: ナビゲーションの概要
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dcbc8f6737c2b7450e42ed01a752087d6e9034c1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259147"
---
# <a name="getting-started-navigation"></a>はじめに: ナビゲーション


## <a name="adding-navigation"></a>ナビゲーションの追加

iOS では、アプリのナビゲーション用に **UINavigationController** クラスが用意されています。ビューのプッシュ/ポップ操作を通じて、アプリを定義する **UIViewControllers** の階層を作ることができます。

これに対し、複数のビューを含む Windows 10 アプリは、ナビゲーションのために web サイトのアプローチをさらに活用します。 ユーザーがコントロールをクリックしてページ間を移動し、アプリ内を進むことを考えてみることができます。 詳しくは、「[ナビゲーション デザインの基本](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)」をご覧ください。

Windows 10 アプリでこのナビゲーションを管理する方法の1つは、 [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)クラスを使用することです。 以下のチュートリアルでは実際に試す方法を示しています。

以前に開始したソリューションに戻り、**MainPage.xaml** ファイルを開いて、 **[デザイン]** ビューにボタンを追加します。 このボタンの **Content** プロパティを "Button" から "Go To Page" に変更します。 次に、ボタンの **Click** イベントのハンドラーを、次の図に示すように作成します。 作成方法がわからない場合は、前のセクションのチュートリアルを見直してください (ヒント: **[デザイン]** ビューにあるボタンをダブルクリックします)。

![Visual Studio でのボタンとそのクリック イベントの追加](images/ios-to-uwp/vs-go-to-page.png)

新しいページを追加しましょう。 **[ソリューション]** ビューで、 **[プロジェクト]** メニュー、 **[新しい項目の追加]** の順にタップします。 次の図に示すように、 **[空白のページ]** をタップし、 **[追加]** をタップします。

![Visual Studio での新しいページの追加](images/ios-to-uwp/vs-add-new-page.png)

次に、BlankPage.xaml ファイルにボタンを追加します。 ここでは、AppBarButton コントロールを使い、ボタンに "前に戻る矢印" の画像を設定します。 **[XAML]** ビューで、` <AppBarButton Icon="Back"/>` を `<Grid> </Grid>` 要素の間に追加します。

次に、イベントハンドラーをボタンに追加してみましょう。**デザイン**ビューでコントロールをダブルクリックし、次の図に示すように "AppBarButton\_click" というテキスト Microsoft Visual Studio**追加して**、対応するイベントハンドラーを BlankPage.xaml.cs ファイルに追加して表示します。

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

ページのナビゲーションは、[**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) クラスによって管理されます。 IOS の**UINavigationController**クラスで**Pushviewcontroller**メソッドと**popviewcontroller**メソッドが使用されているため、UWP アプリの**Frame**クラスは[**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate)メソッドと[**GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback)メソッドを提供します。 **Frame** クラスには、名前から推測されるとおりに動作する [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward) というメソッドもあります。

このチュートリアルでは、ナビゲーションを行うたびに BlankPage の新しいインスタンスが作成されます。 (前のインスタンスは自動的に*解放*されます)。 毎回新しいインスタンスが作成されることがないようにするには、BlankPage.xaml.cs ファイル内の BlankPage クラスのコンストラクターに以下のコードを追加します。 これにより、[**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) 動作が有効になります。

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

また、**Frame** クラスの [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize) プロパティを取得または設定すると、キャッシュするナビゲーションの履歴のページ数を管理できます。

ナビゲーションについて詳しくは、「[ナビゲーション](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)」と「[XAML パーソナリティ アニメーションのサンプル](https://code.msdn.microsoft.com/windowsapps/Personality-Animations-3f857919)」をご覧ください。

**注**  JAVASCRIPT と HTML を使用した UWP アプリのナビゲーションの詳細については、「[クイックスタート: 単一ページナビゲーションの使用](https://docs.microsoft.com/previous-versions/windows/apps/hh452768(v=win.10))」を参照してください。
 
### <a name="next-step"></a>次の手順

[はじめに: アニメーション](getting-started-animation.md)

