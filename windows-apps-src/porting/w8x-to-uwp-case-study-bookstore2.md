---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: このケース スタディは、Bookstore1 で説明されている情報に基づいて作成されています。ここでは最初に、グループ化されたデータを SemanticZoom コントロールに表示するユニバーサル 8.1 アプリについて取り上げます。
title: Windows ランタイム 8.x から UWP へのケース スタディ - Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fb3fdd324caa54c6965ae63485b5b4f264980d7e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259129"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows ランタイム 8.x から UWP へのケース スタディ - Bookstore2


このケース スタディは、「[Bookstore1](w8x-to-uwp-case-study-bookstore1.md)」で説明されている情報に基づいて作成されています。ここでは最初に、グループ化されたデータを [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) コントロールに表示するユニバーサル 8.1 アプリについて取り上げます。 ビュー モデルでは、**Author** クラスの各インスタンスが該当する著者によって書かれた書籍のグループを表します。**SemanticZoom** では、著者ごとにグループ化された書籍の一覧を表示したり、縮小して著者のジャンプ リストを表示したりすることができます。 ジャンプ リストを使うと、書籍の一覧をスクロールするよりもすばやく移動することができます。 We walk through the steps of porting the app to a Windows 10 Universal Windows Platform (UWP) app.

**Note**   When opening Bookstore2Universal\_10 in Visual Studio, if you see the message "Visual Studio update required", then follow the steps in [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>ダウンロード

[Download the Bookstore2\_81 Universal 8.1 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81).

[Download the Bookstore2Universal\_10 Windows 10 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10).

## <a name="the-universal-81-app"></a>ユニバーサル 8.1 アプリ

Here’s what Bookstore2\_81—the app that we're going to port—looks like. このアプリでは、著者ごとにグループ化された書籍を表示する [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) を横方向にスクロール (Windows Phone では縦方向にスクロール) します。 このリストを縮小してジャンプ リストを表示し、そこから任意のグループに移動できます。 このアプリには 2 つの重要な機能があります。それらは、グループ化されたデータ ソースを提供するビュー モデルと、そのビュー モデルにバインドされるユーザー インターフェイスです。 As we'll see, both of these pieces port easily from WinRT 8.1 technology to Windows 10.

![bookstore2\-81 on windows, zoomed-in view](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Bookstore2\_81 on Windows, zoomed-in view
 

![bookstore2\-81 on windows, zoomed-out view](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Bookstore2\_81 on Windows, zoomed-out view

![bookstore2\-81 on windows phone, zoomed-in view](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Bookstore2\_81 on Windows Phone, zoomed-in view

![bookstore2\-81 on windows phone, zoomed-out view](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Bookstore2\_81 on Windows Phone, zoomed-out view

##  <a name="porting-to-a-windows10-project"></a>Porting to a Windows 10 project

The Bookstore2\_81 solution is an 8.1 Universal App project. The Bookstore2\_81.Windows project builds the app package for Windows 8.1, and the Bookstore2\_81.WindowsPhone project builds the app package for Windows Phone 8.1. Bookstore2\_81.Shared is the project that contains source code, markup files, and other assets and resources, that are used by both of the other two projects.

Just like with the previous case study, the option we'll take (of the ones described in [If you have a Universal 8.1 app](w8x-to-uwp-root.md)) is to port the contents of the Shared project to a Windows 10 that targets the Universal device family.

最初に、"新しいアプリケーション (Windows ユニバーサル)" プロジェクトを新規作成します。 Name it Bookstore2Universal\_10. These are the files to copy over from Bookstore2\_81 to Bookstore2Universal\_10.

**From the Shared project**

-   Copy the folder containing the book cover image PNG files (the folder is \\Assets\\CoverImages). フォルダーをコピーしたら、**ソリューション エクスプローラー**で **[すべてのファイルを表示]** がオンであることを確認します。 コピーしたフォルダーを右クリックし、 **[プロジェクトに含める]** をクリックします。 このコマンドは、ファイルまたはフォルダーをプロジェクトに "含める" ことを意味します。 ファイルやフォルダーをコピーするたびに、**ソリューション エクスプローラー**で **[更新]** をクリックしてから、ファイルまたはフォルダーをプロジェクトに含めます。 コピー先で置き換えるファイルについては、この手順を実行する必要はありません。
-   Copy the folder containing the view model source file (the folder is \\ViewModel).
-   MainPage.xaml をコピーして、コピー先のファイルを置き換えます。

**From the Windows project**

-   BookstoreStyles.xaml をコピーします。 We'll use this one as a good starting-point because all the resource keys in this file will resolve in a Windows 10 app; some of those in the equivalent WindowsPhone file will not.
-   SeZoUC.xaml と SeZoUC.xaml.cs をコピーします。 このビューの Windows バージョンで作業を始めます。このビューは、幅が広いウィンドウに適していますが、最終的には小型のデバイスで使うことができるように、より幅が狭いウィンドウに合わせて調整します。

Edit the source code and markup files that you just copied and change any references to the Bookstore2\_81 namespace to Bookstore2Universal\_10. これをすばやく行うには、 **[フォルダーを指定して置換]** 機能を使います。 ビュー モデルでも、その他の命令型コードでも、コードを変更する必要はありません。 But, just to make it easier to see which version of the app is running, change the value returned by the **Bookstore2Universal\_10.BookstoreViewModel.AppName** property from "Bookstore2\_81" to "BOOKSTORE2UNIVERSAL\_10".

これで、ビルドして実行することができます。 Here's how our new UWP app looks after having done no work yet to port it to Windows 10.

![デスクトップ デバイスで動作中の、最初のソース コードの変更を加えた Windows 10 アプリ (拡大表示)](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

The Windows 10 app with initial source code changes running on a Desktop device, zoomed-in view

![デスクトップ デバイスで動作中の、最初のソース コードの変更を加えた Windows 10 アプリ (縮小表示)](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

The Windows 10 app with initial source code changes running on a Desktop device, zoomed-out view

ビュー モデル、拡大表示、縮小表示は適切に連携しますが、確認するのが難しいという問題があります。 第 1 の問題は、[**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) がスクロールしないことです。 This is because, in Windows 10, the default style of a [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) causes it to be laid out vertically (and the Windows 10 design guidelines recommend that we use it that way in new and in ported apps). But, horizontal scrolling settings in the custom items panel template that we copied from the Bookstore2\_81 project (which was designed for the 8.1 app) are in conflict with vertical scrolling settings in the Windows 10 default style that is being applied as a result of us having ported to a Windows 10 app. 第 2 の問題は、アプリでは、さまざまなサイズのウィンドウや小型のデバイスで最適なエクスペリエンスを実現するようにユーザー インターフェイスがまだ対応していないことです。 第 3 の問題は、適切なスタイルとブラシがまだ使われていないことです。このため、ほとんどのテキストが表示されていません (縮小表示のためにクリックできるグループ ヘッダーを含む)。 次の 3 つのセクション (「[SemanticZoom と GridView の設計変更](#semanticzoom-and-gridview-design-changes)」、「[アダプティブ UI](#adaptive-ui)」、「[ユニバーサル スタイル設定](#universal-styling)」) では、これら 3 つの問題に対処します。

## <a name="semanticzoom-and-gridview-design-changes"></a>SemanticZoom と GridView の設計変更

The design changes in Windows 10 to the [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) control are described in the section [SemanticZoom changes](w8x-to-uwp-porting-xaml-and-ui.md). これらの変更に対応するための作業は、このセクションでは行いません。

[  **GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) に対する変更については、「[GridView/ListView に関する変更](w8x-to-uwp-porting-xaml-and-ui.md)」のセクションをご覧ください。 これらの変更に対応するために、次に示す調整を行います。

-   SeZoUC.xaml の `ZoomedInItemsPanelTemplate` で、`Orientation="Horizontal"` と `GroupPadding="0,0,0,20"` を設定します。
-   SeZoUC.xaml で、`ZoomedOutItemsPanelTemplate` を削除し、縮小表示からは `ItemsPanel` 属性を削除します。

以上で作業は終了です。

## <a name="adaptive-ui"></a>アダプティブ UI

上記の変更を行うと、SeZoUC.xaml によって示される UI レイアウトは、幅の広いウィンドウでアプリを実行する場合に便利なレイアウトとなります (大型画面を持つデバイスでのみ役立つと考えられます)。 ただし、アプリのウィンドウが狭い場合は (小型のデバイスが該当しますが、大型のデバイスの場合もあります)、Windows Phone ストア アプリで使った UI が間違いなく最適な UI となります。

これを実現するために、アダプティブな Visual State Manager 機能を使うことができます。 Windows Phone ストア アプリで使っていた、より小さいサイズのテンプレートを利用して、既定で UI が幅の狭い状態でレイアウトされるように、視覚要素のプロパティを設定します。 その後で、アプリのウィンドウ幅が特定のサイズ以上になる状況を確認します (このサイズは[有効ピクセル](w8x-to-uwp-porting-xaml-and-ui.md)の単位で測定します)。また、より大きなレイアウトやより幅の広いレイアウトを実現できるように、視覚要素のプロパティを変更します。 これらのプロパティの変更を表示状態として設定し、アダプティブなトリガーを使って、有効ピクセル単位のウィンドウ幅に応じて、その表示状態を適用するかどうかを継続的に監視し判断します。 この場合はウィンドウの幅でトリガーしていますが、ウィンドウの高さでトリガーすることもできます。

この使用事例では、ウィンドウの最小幅は 548 epx が適しています。これは、最も小型のデバイスで幅の広いレイアウトを表示する際に適したサイズであるためです。 通常、電話は 548 epx よりも小さいため、電話のような小型のデバイスでは既定の幅の狭いレイアウトをそのまま使います。 PC では、既定で、ワイド状態への切り替えをトリガーできる十分な幅でウィンドウが開きます。 この状態でウィンドウをドラッグして、250 x 250 サイズの項目が示される 2 列分を表示できる幅の狭いウィンドウにすることができます。 これよりも幅を狭くすると、トリガーが非アクティブ化されます。これにより、幅の広い表示状態が削除され、既定である幅の狭いレイアウトが有効になります。

これら 2 つの異なるレイアウトを実現するためには、どのプロパティを設定 (および変更) する必要があるでしょうか。 そのためには 2 つの方法があり、それぞれの方法で異なるアプローチが必要です。

1.  2 つの [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) コントロールをマークアップ内に配置できます。 One would be a copy of the markup that we were using in the Windows Runtime 8.x app (using [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) controls inside it), and collapsed by default. もう 1 つは Windows Phone ストア アプリで使っていたマークアップのコピー (マークアップ内の [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) コントロールを使っていました) であり、既定では表示されます。 表示状態によって、2 つの **SemanticZoom** コントロールが持つ表示に関するプロパティが切り替えられます。 この方法では、わずかな作業で 2 つのレイアウトを実現できますが、一般的には、高いパフォーマンスを得ることができる手法ではありません。 そのため、この方法を使う場合は、アプリをプロファイリングし、お客様のパフォーマンスの目標を達成できるかどうかを確認する必要があります。
2.  複数の [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) コントロールを含んでいる 1 つの [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) を使うことができます。 2 つのレイアウトを実現するために、幅の広い表示状態について、**ListView** コントロールのプロパティ (コントロールに適用されるテンプレートなど) を変更します。このとき、[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) と同じ方法でこのコントロールがレイアウトされるようにプロパティを変更します。 この方法では、より優れたパフォーマンスを得ることができる場合がありますが、**GridView** と **ListView** のさまざまなスタイルやテンプレートの間、およびこれらのコントロールにおけるさまざまな項目の種類の間に、細かな相違点が多数あります。このため、2 つのレイアウトを実現するには難しい解決策でもあります。 またこの解決策は、既定のスタイルとテンプレートに関する現時点での設計方法と緊密に関連しており、既定の設定に対して今後加えられる変更の影響を受けやすい、安定性に欠ける解決策でもあります。

このケース スタディでは、最初の方法を採用します。 2 番目の方法を試したい場合は、その方法が適切であるかどうかを確認してください。 最初の方法を実装するための手順を次に示します。

-   新しいプロジェクトのマークアップに含まれている [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) で、`x:Name="wideSeZo"` と `Visibility="Collapsed"` を設定します。
-   Go back to the Bookstore2\_81.WindowsPhone project and open SeZoUC.xaml. このファイルから [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 要素のマークアップをコピーし、新しいプロジェクトの `wideSeZo` の直後に貼り付けます。 貼り付けた要素に対して `x:Name="narrowSeZo"` を設定します。
-   ただし、`narrowSeZo` には、まだコピーしていないスタイルがいくつか必要です。 Again in Bookstore2\_81.WindowsPhone, copy the two styles (`AuthorGroupHeaderContainerStyle` and `ZoomedOutAuthorItemContainerStyle`) out of SeZoUC.xaml and paste them into BookstoreStyles.xaml in your new project.
-   これで、新しい SeZoUC.xaml には 2 つの [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 要素が指定されます。 これら 2 つの要素を **Grid** でラップします。
-   新しいプロジェクトの BookstoreStyles.xaml で、`Wide` という単語を、`AuthorGroupHeaderTemplate`、`ZoomedOutAuthorTemplate`、`BookTemplate` の 3 つのリソース キーに付加します (SeZoUC.xaml に含まれているこれらのリソース キーの参照にもこの語を付加しますが、`wideSeZo` 内にある参照にのみ付加します)。
-   In the Bookstore2\_81.WindowsPhone project, open BookstoreStyles.xaml. From this file, copy those same three resources (mentioned above), and the two jump list item converters, and the namespace prefix declaration Windows\_UI\_Xaml\_Controls\_Primitives, and paste them all into BookstoreStyles.xaml in your new project.
-   最後に、新しいプロジェクトの SeZoUC.xaml で、適切な Visual State Manager のマークアップを上で追加した **Grid** に追加します。

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>ユニバーサル スタイル設定

次に、スタイルの問題を解決します。以前のプロジェクトからコピーするときに、上で説明した問題も含みます。

-   MainPage.xaml で、`LayoutRoot` の Background を `"{ThemeResource ApplicationPageBackgroundThemeBrush}"` に変更します。
-   BookstoreStyles.xaml で、リソース `TitlePanelMargin` の値を `0` (または適切な外観になる任意の値) に設定します。
-   SeZoUC.xaml で、`wideSeZo` の Margin を `0` (または適切な外観になる任意の値) に設定します。
-   BookstoreStyles.xaml で、`AuthorGroupHeaderTemplateWide` から Margin 属性を削除します。
-   `AuthorGroupHeaderTemplate` と `ZoomedOutAuthorTemplate` から、FontFamily 属性を削除します。
-   Bookstore2\_81 used the `BookTemplateTitleTextBlockStyle`, `BookTemplateAuthorTextBlockStyle`, and `PageTitleTextBlockStyle` resource keys as an indirection so that a single key had different implementations in the two apps. その間接参照は、必要ではなくなりました。システム スタイルを直接参照できます。 そのため、アプリ全体でこれらの参照をそれぞれ、`TitleTextBlockStyle`、`CaptionTextBlockStyle`、`HeaderTextBlockStyle` に置き換えます。 Visual Studio の **[フォルダーを指定して置換]** 機能を使って、この操作をすばやく正確に行うことができます。 その後で、使わなくなった 3 つのリソースを削除できます。
-   `AuthorGroupHeaderTemplate` で、`PhoneAccentBrush` を `SystemControlBackgroundAccentBrush` に置き換え、**TextBlock** に対して `Foreground="White"` を設定します。これにより、モバイル デバイス ファミリで実行したときに適切に表示されます。
-   `BookTemplateWide` で、2 番目の **TextBlock** から 1 番目のものに Foreground 属性をコピーします。
-   `ZoomedOutAuthorTemplateWide` で、`SubheaderTextBlockStyle` (今では少し大きすぎる) への参照を `SubtitleTextBlockStyle` への参照に変更します。
-   縮小表示 (ジャンプ リスト) は、新しいプラットフォームでの拡大表示ビューをオーバーレイしません。このため、`narrowSeZo` の縮小表示から `Background` 属性を削除できます。
-   すべてのスタイルとテンプレートを 1 つのファイルに格納するために、SeZoUC.xaml から BookstoreStyles.xaml に `ZoomedInItemsPanelTemplate` を移動します。

スタイル設定操作の最後のシーケンスで、アプリの外観は次のようになります。

![デスクトップ デバイスで動作中の、移植された Windows 10 アプリ (2 つのサイズのウィンドウによる拡大表示)](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

The ported Windows 10 app running on a Desktop device, zoomed-in view, two sizes of window

![デスクトップ デバイスで動作中の、移植された Windows 10 アプリ (2 つのサイズのウィンドウによる縮小表示)](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

The ported Windows 10 app running on a Desktop device, zoomed-out view, two sizes of window

![モバイル デバイスで動作中の、移植された Windows 10 アプリ (拡大表示)](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

The ported Windows 10 app running on a Mobile device, zoomed-in view

![モバイル デバイスで動作中の、移植された Windows 10 アプリ (縮小表示)](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

The ported Windows 10 app running on a Mobile device, zoomed-out view

## <a name="conclusion"></a>まとめ

このケース スタディには、前のケース スタディよりも複雑なユーザー インターフェイスが関連しています。 前のケース スタディと同じように、この特定のビュー モデルでは特に作業を行わなくても、最初はユーザー インターフェイスがリファクタリングされました。 変更が必要となるのは、2 つのプロジェクトを 1 つのプロジェクトにまとめ、多くのフォーム ファクターをサポートする場合です (実際に、前のケース スタディよりも多くのフォーム ファクターを使いました)。 一部の変更点は、プラットフォームに加えられた変更に関連するものでした。

次のケース スタディは「[QuizGame](w8x-to-uwp-case-study-quizgame.md)」です。ここでは、グループ化されたデータへのアクセスと表示について説明します。
