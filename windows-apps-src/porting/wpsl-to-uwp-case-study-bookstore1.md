---
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: This topic presents a case study of porting a very simple Windows Phone Silverlight app to a Windows 10 Universal Windows Platform (UWP) app.
title: Windows Phone Silverlight to UWP case study, Bookstore1
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1079d51aa0b013cd40ff585e5baabb61f940a745
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260094"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore1"></a>Windows Phone Silverlight to UWP case study: Bookstore1


This topic presents a case study of porting a very simple Windows Phone Silverlight app to a Windows 10 Universal Windows Platform (UWP) app. With Windows 10, you can create a single app package that your customers can install onto a wide range of devices, and that's what we'll do in this case study. 「[UWP アプリのガイド](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)」をご覧ください。

移植するアプリは、ビュー モデルにバインドされた **ListBox** で構成されます。 ビュー モデルにはタイトル、著者、表紙を示す書籍の一覧が含まれます。 表紙画像では、 **[ビルド アクション]** が **[コンテンツ]** に設定され、 **[出力ディレクトリにコピー]** が **[コピーしない]** に設定されています。

このセクションの前のトピックでは、プラットフォーム間の違いについて説明し、ビュー モデルへのバインドを通じて、データへのアクセスに至るまで、XAML マークアップからのアプリのさまざまな要素に対する移植プロセスの詳細とガイダンスを提供しました。 ケース スタディでは、実際の例が動作するようすを示すことにより、このガイダンスを補足することを目的としています。 ケース スタディは、ガイダンスを読み終わっていることを前提としているため、繰り返し説明することはありません。

**Note**   When opening Bookstore1Universal\_10 in Visual Studio, if you see the message "Visual Studio update required", then follow the steps for selecting a Target Platform Versioning in [TargetPlatformVersion](wpsl-to-uwp-troubleshooting.md).

## <a name="downloads"></a>ダウンロード

[Download the Bookstore1WPSL8 Windows Phone Silverlight app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1WPSL8).

[Download the Bookstore1Universal\_10 Windows 10 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).

## <a name="the-windowsphone-silverlight-app"></a>The Windows Phone Silverlight app

次に、ここで移植するアプリ Bookstore1WPSL8 の外観を示します。 これは、アプリ名とページ タイトルの見出しの下に、縦方向にスクロールする書籍のリスト ボックスを示すシンプルなアプリです。

![bookstore1wpsl8 の外観](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows10-project"></a>Porting to a Windows 10 project

Visual Studio で新しいプロジェクトを作成し、そこへ Bookstore1WPSL8 からファイルをコピーし、コピーしたファイルを新しいプロジェクトに含めるというタスクは、非常に短時間で実行できます。 最初に、"新しいアプリケーション (Windows ユニバーサル)" プロジェクトを新規作成します。 Name it Bookstore1Universal\_10. These are the files to copy over from Bookstore1WPSL8 to Bookstore1Universal\_10.

-   Copy the folder containing the book cover image PNG files (the folder is \\Assets\\CoverImages). フォルダーをコピーしたら、**ソリューション エクスプローラー**で **[すべてのファイルを表示]** がオンであることを確認します。 コピーしたフォルダーを右クリックし、 **[プロジェクトに含める]** をクリックします。 このコマンドは、ファイルまたはフォルダーをプロジェクトに "含める" ことを意味します。 ファイルやフォルダーをコピーするたびに、**ソリューション エクスプローラー**で **[更新]** をクリックしてから、ファイルまたはフォルダーをプロジェクトに含めます。 コピー先で置き換えるファイルについては、この手順を実行する必要はありません。
-   Copy the folder containing the view model source file (the folder is \\ViewModel).
-   MainPage.xaml をコピーして、コピー先のファイルを置き換えます。

We can keep the App.xaml, and App.xaml.cs that Visual Studio generated for us in the Windows 10 project.

Edit the source code and markup files that you just copied and change any references to the Bookstore1WPSL8 namespace to Bookstore1Universal\_10. これをすばやく行うには、 **[フォルダーを指定して置換]** 機能を使います。 ビュー モデルのソース ファイルに含まれている命令型コードでは、移植作業のために次の変更を行う必要があります。

-   `System.ComponentModel.DesignerProperties` を `DesignMode` に変更した後、これに対して **[解決]** コマンドを使います。 `IsInDesignTool` プロパティを削除し、IntelliSense を使って適切なプロパティ名 (`DesignModeEnabled`) を追加します。
-   `ImageSource` に対して **[解決]** コマンドを使います。
-   `BitmapImage` に対して **[解決]** コマンドを使います。
-   using `System.Windows.Media;` と `using System.Windows.Media.Imaging;` を削除します。
-   Change the value returned by the **Bookstore1Universal\_10.BookstoreViewModel.AppName** property from "BOOKSTORE1WPSL8" to "BOOKSTORE1UNIVERSAL".

MainPage.xaml では、移植作業のために次の変更を行う必要があります。

-   `phone:PhoneApplicationPage` を `Page` に変更します (プロパティ要素構文での使用回数を考慮してください)。
-   `phone` と `shell` の名前空間のプレフィックス宣言を削除します。
-   その他の名前空間のプレフィックス宣言で、"clr-namespace" を "using" に変更します。

一時的にマークアップを削除することになっても、すぐに結果を確認したい場合は、マークアップ コンパイル エラーを非常に単純に修正することもできます。 ただしここでは、そうすることでとりあえず先に進み、後で見直すことにしましょう。 次に、このための手順を示します。

1.  **MainPage.xaml** のルート **Page** 要素で、`SupportedOrientations="Portrait"` を削除します。
2.  **MainPage.xaml** のルート **Page** 要素で、`Orientation="Portrait"` を削除します。
3.  **MainPage.xaml** のルート **Page** 要素で、`shell:SystemTray.IsVisible="True"` を削除します。
4.  `BookTemplate` データ テンプレートで、`PhoneTextExtraLargeStyle` および `PhoneTextSubtleStyle` の  **TextBlock** スタイルへの参照を削除します。
5.  `TitlePanel`   **StackPanel** で、`PhoneTextNormalStyle` および `PhoneTextTitle1Style` の  **TextBlock** スタイルへの参照を削除します。

最初にモバイル デバイス ファミリの UI の作業を行い、その後でその他のフォーム ファクターについて検討します。 これで、アプリをビルドして実行できるようになりました。 モバイル エミュレーターでは次のように表示されます。

![最初のソース コードの変更を加えたモバイルの UWP アプリ](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

ビューおよびビュー モデルは、適切に連携しており、**ListBox** が機能しています。 ほとんどの場合、若干スタイルを修正して、画像を表示する必要があります。

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>削除した項目と一部の初期スタイルを戻す

既定では、すべての向きがサポートされます。 The Windows Phone Silverlight app explicitly constrains itself to portrait-only, though, so debt items \#1 and \#2 are paid off by going into the app package manifest in the new project and checking **Portrait** under **Supported orientations**.

For this app, item \#3 is not a debt since the status bar (formerly called the system tray) is shown by default. For items \#4 and \#5, we need to find four Universal Windows Platform (UWP) **TextBlock** styles that correspond to the Windows Phone Silverlight styles that we were using. You can run the Windows Phone Silverlight app in the emulator and compare it side-by-side with the illustration in the [Text](wpsl-to-uwp-porting-xaml-and-ui.md) section. From doing that, and from looking at the properties of the Windows Phone Silverlight system styles, we can make this table.

| Windows Phone Silverlight スタイル キー | UWP スタイル キー          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |
 
こうしたスタイルを設定するために、マークアップ エディターに単純に入力するか、Visual Studio XAML ツールを使えば入力なしで設定できます。 To do that, you right-click a **TextBlock** and click **Edit Style** &gt; **Apply Resource**. To do that with the **TextBlock**s in the item template, right click the **ListBox** and click **Edit Additional Templates** &gt; **Edit Generated Items (ItemTemplate)** .

**ListBox** コントロールの既定のスタイルでは背景に `ListBoxBackgroundThemeBrush` システム リソースが設定されるため、項目の背景は 80% の不透明な白になります。 **ListBox** で `Background="Transparent"` を設定し、背景をクリアします。 項目テンプレートで **TextBlock** を左揃えにするには、前記と同様に再度編集し、両方の **TextBlock** で `"9.6,0"` の **Margin** を設定します。

これが終わったら、[表示ピクセルに関連する変更](wpsl-to-uwp-porting-xaml-and-ui.md)のために、まだ変更していないすべての固定サイズの寸法 (余白、幅、高さなど) について、0.8 を乗算する必要があります。 したがって、たとえば画像は 70 x 70px から 56 x 56px に変更する必要があります。

ただし、スタイルの結果を示す前に、画像を描画しましょう。

## <a name="binding-an-image-to-a-view-model"></a>ビュー モデルへの画像のバインド

Bookstore1WPSL8 では、次のことを適用しました。

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

Bookstore1Universal では、ms-appx [URI スキーム](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10))を使います。 残るコードを変更せずに維持するために、**System.Uri** コンストラクターの異なるオーバーロードを使って、ベース URI に ms-appx URI スキームを格納し、パスの残る部分を追加できます。 次に例を示します。

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>ユニバーサル スタイル設定

後は、いくつかの最終的なスタイルの調整を行い、アプリがデスクトップ (およびその他) のフォーム ファクターとモバイルで適切に表示されることを確認するだけです。 手順は次のとおりです。 このトピックの上部にあるリンクを使用して、プロジェクトをダウンロードし、この時点とケース スタディの終了時の間のすべての変更の結果を参照できます。

-   項目間のスペースを縮めるために、MainPage.xaml で `BookTemplate` データ テンプレートを探し、`Margin` 属性をルート **Grid** から削除します。
-   ページ タイトルに少しゆとりを与える場合は、`-5.6` の下部の余白をページ タイトル **TextBlock** で `0` に設定します。
-   ここで、`LayoutRoot` の Background を適切な既定値に設定して、テーマが何であるかに関係なく、すべてのデバイスでの実行時にアプリが適切に表示されるようにする必要があります。 これを、`"Transparent"` から `"{ThemeResource ApplicationPageBackgroundThemeBrush}"` に変更します。

ここで、より洗練されたアプリにより、「[フォーム ファクターとユーザー エクスペリエンスのための移植](wpsl-to-uwp-form-factors-and-ux.md)」のガイダンスを参考にして、実際にアプリで実行できる多くのデバイスのそれぞれで、フォーム ファクターを最適に利用します。 このシンプルなアプリでは、ここで停止し、スタイル操作の最後の手順を行った後のアプリの外観を確認します。 実際には、モバイル デバイスとデスクトップ デバイスで同じに表示されますが、広いフォーム ファクターの領域を最大限に活用していません (ただし、後のケース スタディで、これを行う方法を調査します)。

アプリのテーマの管理方法については、「[テーマの変更](wpsl-to-uwp-porting-xaml-and-ui.md)」をご覧ください。

![移植された Windows 10 アプリ](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

The ported Windows 10 app running on a Mobile device

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>モバイル デバイス向けのリスト ボックスの調整 (オプション)

モバイル デバイスでのアプリの実行中には、両方のテーマで既定で、リスト ボックスの背景は淡色です。 このスタイルで問題なければ、それ以上の操作は必要ありません。 コントロールは、動作に影響を及ぼすことなく、表示形式をカスタマイズできるように設計されています。 元のアプリの外観のように、濃色のテーマでリスト ボックスを濃色にする場合は、[このページ](w8x-to-uwp-case-study-bookstore1.md)の「モバイル デバイス向けのリスト ボックスの調整 (オプション)」の手順に従います。

## <a name="conclusion"></a>まとめ

このケース スタディでは、非常に単純なアプリ (実在しないと考えらえる単純なアプリ) を移植するプロセスを示しました。 たとえば、リスト コントロールは、選択用またはナビゲーション コンテキストの確立用に使うことができます。アプリは、タップされた項目に関する詳細を提示するページに移動します。 この特定のアプリは、ユーザーの選択に対して何も処理せず、またナビゲーション機能がありません。 それでも、このケース スタディは、まず移植プロセスを導入し、実際の UWP  アプリで使うことができる重要な手法をデモする役割を果たします。

次のケース スタディは「[Bookstore2](wpsl-to-uwp-case-study-bookstore2.md)」です。ここでは、グループ化されたデータへのアクセスと表示について説明します。
