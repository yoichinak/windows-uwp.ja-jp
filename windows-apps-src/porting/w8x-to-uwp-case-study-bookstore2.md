---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: このケース スタディは、Bookstore1 で説明されている情報に基づいて作成されています。ここでは最初に、グループ化されたデータを SemanticZoom コントロールに表示するユニバーサル 8.1 アプリについて取り上げます。
title: Windows ランタイム 8.x から UWP へのケース スタディ - Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fb3fdd324caa54c6965ae63485b5b4f264980d7e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259129"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows ランタイム 8.x から UWP へのケース スタディ - Bookstore2


このケース スタディは、「[Bookstore1](w8x-to-uwp-case-study-bookstore1.md)」で説明されている情報に基づいて作成されています。ここでは最初に、グループ化されたデータを [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) コントロールに表示するユニバーサル 8.1 アプリについて取り上げます。 ビュー モデルでは、**Author** クラスの各インスタンスが該当する著者によって書かれた書籍のグループを表します。**SemanticZoom** では、著者ごとにグループ化された書籍の一覧を表示したり、縮小して著者のジャンプ リストを表示したりすることができます。 ジャンプ リストを使うと、書籍の一覧をスクロールするよりもすばやく移動することができます。 ここでは、アプリを Windows 10 ユニバーサル Windows プラットフォーム (UWP) アプリに移植する手順について説明します。

Visual Studio で Bookstore2Universal\_10 を開いたときに、"Visual Studio 更新プログラムが必要です" というメッセージが表示された場合は、 [Targetplatformversion](w8x-to-uwp-troubleshooting.md)の手順に従っ**て   ます**。

## <a name="downloads"></a>ダウンロード

[Bookstore2\_81 Universal 8.1 アプリをダウンロード](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81)します。

[Bookstore2Universal\_10 Windows 10 アプリをダウンロード](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10)します。

## <a name="the-universal-81-app"></a>ユニバーサル 8.1 アプリ

ここでは、Bookstore2\_81 (ポートに送信されるアプリ) を示します。 このアプリでは、著者ごとにグループ化された書籍を表示する [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) を横方向にスクロール (Windows Phone では縦方向にスクロール) します。 このリストを縮小してジャンプ リストを表示し、そこから任意のグループに移動できます。 このアプリには 2 つの重要な機能があります。それらは、グループ化されたデータ ソースを提供するビュー モデルと、そのビュー モデルにバインドされるユーザー インターフェイスです。 ご覧のとおり、これらの両方が WinRT 8.1 テクノロジから Windows 10 に簡単に移植できます。

![windows 上の bookstore2\-81、拡大表示](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Windows 上の Bookstore2\_81、拡大表示
 

![windows での bookstore2\-81、縮小表示](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Windows での Bookstore2\_81、縮小表示

![windows phone での bookstore2\-81、拡大表示](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Windows Phone での Bookstore2\_81、拡大表示

![windows phone での bookstore2\-81、縮小表示](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Windows Phone での Bookstore2\_81、縮小表示

##  <a name="porting-to-a-windows10-project"></a>Windows 10 プロジェクトへの移植

Bookstore2\_81 ソリューションは、8.1 ユニバーサルアプリプロジェクトです。 Bookstore2\_81. Windows プロジェクトは Windows 8.1 用のアプリケーションパッケージをビルドし、Bookstore2\_81. Appname.windowsphone プロジェクトは Windows Phone 8.1 のアプリパッケージをビルドします。 Bookstore2\_81. Shared は、他の2つのプロジェクトの両方で使用されるソースコード、マークアップファイル、およびその他のアセットとリソースを含むプロジェクトです。

前のケーススタディと同じように、( [universal 8.1 アプリを](w8x-to-uwp-root.md)使用している場合) で行うオプションは、共有プロジェクトのコンテンツをユニバーサルデバイスファミリをターゲットとする Windows 10 に移植することです。

最初に、"新しいアプリケーション (Windows ユニバーサル)" プロジェクトを新規作成します。 「Bookstore2Universal\_10」という名前を指定します。 これらは、Bookstore2\_81 から Bookstore2Universal\_10 にコピーするファイルです。

**共有プロジェクトから**

-   ブックのカバー画像 PNG ファイルが格納されているフォルダーをコピーします (このフォルダーは \\アセット\\カバーイメージ)。 フォルダーをコピーしたら、**ソリューション エクスプローラー**で **[すべてのファイルを表示]** がオンであることを確認します。 コピーしたフォルダーを右クリックし、 **[プロジェクトに含める]** をクリックします。 このコマンドは、ファイルまたはフォルダーをプロジェクトに "含める" ことを意味します。 ファイルやフォルダーをコピーするたびに、**ソリューション エクスプローラー**で **[更新]** をクリックしてから、ファイルまたはフォルダーをプロジェクトに含めます。 コピー先で置き換えるファイルについては、この手順を実行する必要はありません。
-   ビューモデルのソースファイルが格納されているフォルダーをコピーします (このフォルダーは \\ビューモデルです)。
-   MainPage.xaml をコピーして、コピー先のファイルを置き換えます。

**Windows プロジェクトから**

-   BookstoreStyles.xaml をコピーします。 このファイル内のすべてのリソースキーは Windows 10 アプリで解決されるため、この方法をお勧めします。同等の Appname.windowsphone ファイルには含まれません。
-   SeZoUC.xaml と SeZoUC.xaml.cs をコピーします。 このビューの Windows バージョンで作業を始めます。このビューは、幅が広いウィンドウに適していますが、最終的には小型のデバイスで使うことができるように、より幅が狭いウィンドウに合わせて調整します。

コピーしたソースコードとマークアップファイルを編集し、Bookstore2\_81 名前空間への参照を Bookstore2Universal\_10 に変更します。 これをすばやく行うには、 **[フォルダーを指定して置換]** 機能を使います。 ビュー モデルでも、その他の命令型コードでも、コードを変更する必要はありません。 しかし、どのバージョンのアプリが実行されているかを簡単に確認できるように、 **Bookstore2Universal\_10. BookstoreViewModel. AppName** 81\_プロパティによって返される値を "Bookstore2Universal\_10" に変更します。

これで、ビルドして実行することができます。 Windows 10 に移植するための作業がまだ完了していない場合は、次のように新しい UWP アプリが表示されます。

![デスクトップ デバイスで動作中の、最初のソース コードの変更を加えた Windows 10 アプリ (拡大表示)](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

デスクトップデバイスで実行されている最初のソースコードの変更を含む Windows 10 アプリ (拡大表示)

![デスクトップ デバイスで動作中の、最初のソース コードの変更を加えた Windows 10 アプリ (縮小表示)](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

デスクトップデバイスで実行されている最初のソースコードの変更を含む Windows 10 アプリの縮小表示

ビュー モデル、拡大表示、縮小表示は適切に連携しますが、確認するのが難しいという問題があります。 第 1 の問題は、[**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) がスクロールしないことです。 これは、Windows 10 では[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)の既定のスタイルによって垂直方向にレイアウトされるためです (windows 10 の設計ガイドラインでは、新しいおよび移植されたアプリでの使用を推奨しています)。 ただし、Bookstore2\_81 プロジェクト (8.1 アプリ向けに設計されています) からコピーした [カスタム項目] パネルテンプレートの水平スクロールの設定は、Windows 10 アプリに移植した結果として適用されている Windows 10 の既定のスタイルの垂直スクロール設定と競合しています。 第 2 の問題は、アプリでは、さまざまなサイズのウィンドウや小型のデバイスで最適なエクスペリエンスを実現するようにユーザー インターフェイスがまだ対応していないことです。 第 3 の問題は、適切なスタイルとブラシがまだ使われていないことです。このため、ほとんどのテキストが表示されていません (縮小表示のためにクリックできるグループ ヘッダーを含む)。 次の 3 つのセクション (「[SemanticZoom と GridView の設計変更](#semanticzoom-and-gridview-design-changes)」、「[アダプティブ UI](#adaptive-ui)」、「[ユニバーサル スタイル設定](#universal-styling)」) では、これら 3 つの問題に対処します。

## <a name="semanticzoom-and-gridview-design-changes"></a>SemanticZoom と GridView の設計変更

[**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)コントロールに対する Windows 10 のデザイン変更については、「 [SemanticZoom changes](w8x-to-uwp-porting-xaml-and-ui.md)」セクションを参照してください。 これらの変更に対応するための作業は、このセクションでは行いません。

[  **GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) に対する変更については、「[GridView/ListView に関する変更](w8x-to-uwp-porting-xaml-and-ui.md)」のセクションをご覧ください。 これらの変更に対応するために、次に示す調整を行います。

-   SeZoUC.xaml の `ZoomedInItemsPanelTemplate` で、`Orientation="Horizontal"` と `GroupPadding="0,0,0,20"` を設定します。
-   SeZoUC.xaml で、`ZoomedOutItemsPanelTemplate` を削除し、縮小表示からは `ItemsPanel` 属性を削除します。

以上で作業は終了です。

## <a name="adaptive-ui"></a>アダプティブ UI

上記の変更を行うと、SeZoUC.xaml によって示される UI レイアウトは、幅の広いウィンドウでアプリを実行する場合に便利なレイアウトとなります (大型画面を持つデバイスでのみ役立つと考えられます)。 ただし、アプリのウィンドウが狭い場合は (小型のデバイスが該当しますが、大型のデバイスの場合もあります)、Windows Phone ストア アプリで使った UI が間違いなく最適な UI となります。

これを実現するために、アダプティブな Visual State Manager 機能を使うことができます。 Windows Phone ストア アプリで使っていた、より小さいサイズのテンプレートを利用して、既定で UI が幅の狭い状態でレイアウトされるように、視覚要素のプロパティを設定します。 その後で、アプリのウィンドウ幅が特定のサイズ以上になる状況を確認します (このサイズは[有効ピクセル](w8x-to-uwp-porting-xaml-and-ui.md)の単位で測定します)。また、より大きなレイアウトやより幅の広いレイアウトを実現できるように、視覚要素のプロパティを変更します。 これらのプロパティの変更を表示状態として設定し、アダプティブなトリガーを使って、有効ピクセル単位のウィンドウ幅に応じて、その表示状態を適用するかどうかを継続的に監視し判断します。 この場合はウィンドウの幅でトリガーしていますが、ウィンドウの高さでトリガーすることもできます。

この使用事例では、ウィンドウの最小幅は 548 epx が適しています。これは、最も小型のデバイスで幅の広いレイアウトを表示する際に適したサイズであるためです。 通常、電話は 548 epx よりも小さいため、電話のような小型のデバイスでは既定の幅の狭いレイアウトをそのまま使います。 PC では、既定で、ワイド状態への切り替えをトリガーできる十分な幅でウィンドウが開きます。 この状態でウィンドウをドラッグして、250 x 250 サイズの項目が示される 2 列分を表示できる幅の狭いウィンドウにすることができます。 これよりも幅を狭くすると、トリガーが非アクティブ化されます。これにより、幅の広い表示状態が削除され、既定である幅の狭いレイアウトが有効になります。

これら 2 つの異なるレイアウトを実現するためには、どのプロパティを設定 (および変更) する必要があるでしょうか。 そのためには 2 つの方法があり、それぞれの方法で異なるアプローチが必要です。

1.  2 つの [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) コントロールをマークアップ内に配置できます。 1つは、Windows ランタイム 8. x アプリで ( [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)コントロールを使用して) 使用していたマークアップのコピーであり、既定では折りたたまれています。 もう 1 つは Windows Phone ストア アプリで使っていたマークアップのコピー (マークアップ内の [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) コントロールを使っていました) であり、既定では表示されます。 表示状態によって、2 つの **SemanticZoom** コントロールが持つ表示に関するプロパティが切り替えられます。 この方法では、わずかな作業で 2 つのレイアウトを実現できますが、一般的には、高いパフォーマンスを得ることができる手法ではありません。 そのため、この方法を使う場合は、アプリをプロファイリングし、お客様のパフォーマンスの目標を達成できるかどうかを確認する必要があります。
2.  複数の [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) コントロールを含んでいる 1 つの [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) を使うことができます。 2 つのレイアウトを実現するために、幅の広い表示状態について、**ListView** コントロールのプロパティ (コントロールに適用されるテンプレートなど) を変更します。このとき、[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) と同じ方法でこのコントロールがレイアウトされるようにプロパティを変更します。 この方法では、より優れたパフォーマンスを得ることができる場合がありますが、**GridView** と **ListView** のさまざまなスタイルやテンプレートの間、およびこれらのコントロールにおけるさまざまな項目の種類の間に、細かな相違点が多数あります。このため、2 つのレイアウトを実現するには難しい解決策でもあります。 またこの解決策は、既定のスタイルとテンプレートに関する現時点での設計方法と緊密に関連しており、既定の設定に対して今後加えられる変更の影響を受けやすい、安定性に欠ける解決策でもあります。

このケース スタディでは、最初の方法を採用します。 2 番目の方法を試したい場合は、その方法が適切であるかどうかを確認してください。 最初の方法を実装するための手順を次に示します。

-   新しいプロジェクトのマークアップに含まれている [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) で、`x:Name="wideSeZo"` と `Visibility="Collapsed"` を設定します。
-   Bookstore2\_81. Appname.windowsphone プロジェクトに戻り、SeZoUC .xaml を開きます。 このファイルから [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 要素のマークアップをコピーし、新しいプロジェクトの `wideSeZo` の直後に貼り付けます。 貼り付けた要素に対して `x:Name="narrowSeZo"` を設定します。
-   ただし、`narrowSeZo` には、まだコピーしていないスタイルがいくつか必要です。 Bookstore2\_81. Appname.windowsphone でも、2つのスタイル (`AuthorGroupHeaderContainerStyle` と `ZoomedOutAuthorItemContainerStyle`) を SeZoUC .xaml からコピーして、新しいプロジェクトの BookstoreStyles .xaml に貼り付けます。
-   これで、新しい SeZoUC.xaml には 2 つの [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 要素が指定されます。 これら 2 つの要素を **Grid** でラップします。
-   新しいプロジェクトの BookstoreStyles.xaml で、`Wide` という単語を、`wideSeZo`、`AuthorGroupHeaderTemplate`、`ZoomedOutAuthorTemplate` の 3 つのリソース キーに付加します (SeZoUC.xaml に含まれているこれらのリソース キーの参照にもこの語を付加しますが、`BookTemplate` 内にある参照にのみ付加します)。
-   Bookstore2\_81. Appname.windowsphone プロジェクトで、BookstoreStyles. .xaml を開きます。 このファイルから、(前述のように) 同じ3つのリソースと2つのジャンプリスト項目のコンバーターをコピーします。また、名前空間プレフィックス宣言 Windows\_UI\_\_Xaml によって\_プリミティブが制御され、すべてが新しいプロジェクトの BookstoreStyles に貼り付けられます。
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
-   Bookstore2\_81 では、`BookTemplateTitleTextBlockStyle`、`BookTemplateAuthorTextBlockStyle`、および `PageTitleTextBlockStyle` リソースキーを間接参照として使用して、1つのキーが2つのアプリで異なる実装を持つようにしました。 その間接参照は、必要ではなくなりました。システム スタイルを直接参照できます。 そのため、アプリ全体でこれらの参照をそれぞれ、`TitleTextBlockStyle`、`CaptionTextBlockStyle`、`HeaderTextBlockStyle` に置き換えます。 Visual Studio の **[フォルダーを指定して置換]** 機能を使って、この操作をすばやく正確に行うことができます。 その後で、使わなくなった 3 つのリソースを削除できます。
-   `AuthorGroupHeaderTemplate` で、`PhoneAccentBrush` を `SystemControlBackgroundAccentBrush` に置き換え、`Foreground="White"`TextBlock**に対して** を設定します。これにより、モバイル デバイス ファミリで実行したときに適切に表示されます。
-   `BookTemplateWide` で、2 番目の **TextBlock** から 1 番目のものに Foreground 属性をコピーします。
-   `ZoomedOutAuthorTemplateWide` で、`SubheaderTextBlockStyle` (今では少し大きすぎる) への参照を `SubtitleTextBlockStyle` への参照に変更します。
-   縮小表示 (ジャンプ リスト) は、新しいプラットフォームでの拡大表示ビューをオーバーレイしません。このため、`Background` の縮小表示から `narrowSeZo` 属性を削除できます。
-   すべてのスタイルとテンプレートを 1 つのファイルに格納するために、SeZoUC.xaml から BookstoreStyles.xaml に `ZoomedInItemsPanelTemplate` を移動します。

スタイル設定操作の最後のシーケンスで、アプリの外観は次のようになります。

![デスクトップ デバイスで動作中の、移植された Windows 10 アプリ (2 つのサイズのウィンドウによる拡大表示)](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

デスクトップデバイスで実行される移植された Windows 10 アプリ、拡大ビュー、2つのサイズのウィンドウ

![デスクトップ デバイスで動作中の、移植された Windows 10 アプリ (2 つのサイズのウィンドウによる縮小表示)](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

デスクトップデバイスで実行される移植された Windows 10 アプリ、ズームアウトビュー、ウィンドウの2つのサイズ

![モバイル デバイスで動作中の、移植された Windows 10 アプリ (拡大表示)](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

モバイルデバイスで実行されている移植された Windows 10 アプリ (拡大表示)

![モバイル デバイスで動作中の、移植された Windows 10 アプリ (縮小表示)](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

モバイルデバイスで実行されている移植された Windows 10 アプリ (縮小表示)

## <a name="conclusion"></a>まとめ

このケース スタディには、前のケース スタディよりも複雑なユーザー インターフェイスが関連しています。 前のケース スタディと同じように、この特定のビュー モデルでは特に作業を行わなくても、最初はユーザー インターフェイスがリファクタリングされました。 変更が必要となるのは、2 つのプロジェクトを 1 つのプロジェクトにまとめ、多くのフォーム ファクターをサポートする場合です (実際に、前のケース スタディよりも多くのフォーム ファクターを使いました)。 一部の変更点は、プラットフォームに加えられた変更に関連するものでした。

次のケース スタディは「[QuizGame](w8x-to-uwp-case-study-quizgame.md)」です。ここでは、グループ化されたデータへのアクセスと表示について説明します。
