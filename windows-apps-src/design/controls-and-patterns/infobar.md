---
description: InfoBar は、アプリ全体の重要なメッセージに関するインライン通知です。
title: InfoBar
template: detail.hbs
ms.date: 11/30/2020
ms.topic: article
keywords: windows 10, winui, uwp
pm-contact: gabilka
design-contact: kimsea
dev-contact: ranjeshj
ms.custom: 20H2
ms.localizationpriority: medium
ms.openlocfilehash: f790e4ed1d16ac42c95f9a835a3b9cc7f3598190
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104543"
---
# <a name="infobar"></a>InfoBar
InfoBar コントロールは、アプリ全体の状態メッセージを、非常に見やすくても邪魔にならないようにユーザーに表示するためのものです。 表示されるメッセージの種類を簡単に示すことができる組み込みの重大度レベルと、アクション ボタンやハイパーリンク ボタンに独自の呼び出しを含めるためのオプションがあります。 InfoBar は他の UI コンテンツとインラインで表示されるため、コントロールにはユーザーがそれを常に表示したり破棄したりできるようにするためのオプションがあります。 

**Windows UI ライブラリを入手する**

:::row:::
   :::column:::
      ![WinUI ロゴ](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **InfoBar** コントロールを使用するには、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリが必要です。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI ライブラリ API:** [InfoBar クラス](/uwp/api/microsoft.ui.xaml.controls.infobar)

> [!TIP]
> このドキュメントでは、XAML で **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 [Page](/uwp/api/windows.ui.xaml.controls.page) 要素にこれを追加しました。`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>コードビハインドでは、C# でも **muxc** エイリアスを使って、プロジェクトに含めた Windows UI Library API を表します。 この **using** ステートメントは、ファイルの先頭に追加されています。`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?
アプリケーションの状態が変化したときに、ユーザーが通知を受け取ったり、確認したり、操作を実行したりする必要があるときは、InfoBar コントロールを使用します。 既定では、通知はユーザーによって閉じられるまでコンテンツ エリアに表示されたままになりますが、必ずしもユーザー フローが中断されることはありません。

InfoBar は、レイアウトの領域を利用し、他の子要素と同じように動作します。 他のコンテンツを覆ったり、その手前にフローティングしたりすることはありません。

アプリの状態が変更されないユーザー操作、時間的制約のあるアラート、または重要ではないメッセージへの直接的な確認や応答には、InfoBar を使用しないでください。

### <a name="remarks"></a>注釈
ユーザーによって閉じられる、またはアプリの認識やエクスペリエンスに **直接的に** 影響するシナリオの状態が解消されたときに閉じられる、InfoBar を使用します。


次に例をいくつか示します。
- インターネット接続の喪失
- 特定のユーザー操作に関係のない、自動的にトリガーされたときのドキュメントの保存中のエラー
- 録音しようとしたときにマイクが接続されていない
- 電話に接続できない
- アプリケーションのサブスクリプションの有効期限が切れる


アプリの認識やエクスペリエンスに **間接的に** 影響するシナリオに、ユーザーによって閉じられる InfoBar を使用します

次に例をいくつか示します。
- 通話で記録が開始された
- "リリース ノート" へのリンクで適用される更新
- サービスの利用規約が更新され、確認が必要である
- アプリ全体のバックアップが正常かつ非同期に完了した
- アプリケーションのサブスクリプションの有効期限が近づいている

### <a name="when-should-a-different-control-be-used"></a>別のコントロールを使用する必要がある場合

[ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、[Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)、または [TeachingTip](/uwp/api/Microsoft.UI.Xaml.Controls.TeachingTip) を使用する方が適切なシナリオもあります。

- 通知を表示し続ける必要がないシナリオ (特定の UI 要素のコンテキストで情報を表示するときなど) の場合は、[Flyout](/uwp/design/controls-and-patterns/dialogs-and-flyouts/flyouts) の方が適切なオプションです。 
- アプリケーションがユーザーの操作を確認するシナリオで、ユーザー読む "**_必要のある_* _" 情報を表示するときは、[ContentDialog](/uwp/design/controls-and-patterns/dialogs-and-flyouts/dialogs) を使用します。
  - さらに、アプリの状態変化が非常に激しく、ユーザーがアプリと対話するためのすべての機能をブロックする必要がある場合は、ContentDialog を使用します。
- 通知が短時間の状態を伝える一時的なものであるシナリオでは、[TeachingTip](/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip) の方が適切なオプションです。


適切な通知コントロールの選択の詳細については、[ダイアログとポップアップ](/uwp/design/controls-and-patterns/dialogs-and-flyouts/)に関する記事を参照してください。


## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/InfoBar">アプリを開き、InfoBar の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-an-infobar"></a>InfoBar を作成する
次の XAML は、情報通知の既定のスタイルを使用するインラインの InfoBar を記述するものです。 情報バーは、他の要素と同じように配置することができ、基本レイアウトの動作に従います。
たとえば、垂直方向の StackPanel で使用すると、InfoBar は横方向に広がり、使用可能な幅いっぱいに表示されます。 


既定では、InfoBar は表示されません。 InfoBar を表示するには、XAML または分離コードで IsOpen プロパティを true に設定します。


```xaml
<muxc:InfoBar x:Name="UpdateAvailableNotification"
    Title="Update available."
    Message="Restart the application to apply the latest update.">
</muxc:InfoBar>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(IsUpdateAvailable())
    {
        UpdateAvailableNotification.IsOpen = true;
    }
}
```

![閉じるボタンとメッセージが表示された、既定の状態の InfoBar のサンプル](images/infobar-default-title-message.png)

### <a name="using-pre-defined-severity-levels"></a>定義済みの重大度レベルの使用
Severity プロパティを使用して情報バーの種類を設定し、通知の重要度に応じて、一貫性のある状態の色、アイコン、支援技術を自動的に設定できます。 Severity が設定されていない場合は、既定の情報スタイルが適用されます。


```xaml
<muxc:InfoBar x:Name="SubscriptionExpiringNotification"
    Severity="Warning"
    Title="Your subscription is expiring in 3 days."
    Message="Renew your subscription to keep all functionality" />
```
![閉じるボタンとメッセージが表示された、警告状態の InfoBar のサンプル](images/infobar-warning-title-message.png)

### <a name="programmatic-close-in-infobar"></a>InfoBar でのプログラムによる終了
InfoBar は、ユーザーが閉じるボタンを使用することで、またはプログラムにより、閉じることができます。 状態が解決されるまで通知を表示したままにする必要があり、ユーザーが情報バーを閉じることができないようにする場合は、IsClosable プロパティを false に設定できます。


このプロパティの既定値は true であり、"X" の形状をした閉じるボタンが表示されます。


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working."
    IsClosable="False" />
```
![閉じるボタンが表示されていないエラー状態の InfoBar のサンプル](images/infobar-error-no-close.png)

### <a name="customization-background-color-and-icon"></a>カスタマイズ: 背景色とアイコン
定義済みの重大度レベル以外では、Background および IconSource プロパティを設定して、アイコンと背景色をカスタマイズできます。 InfoBar の支援技術の設定は、定義されている重要度のままになるか、定義されていない場合は既定値になります。


カスタムの背景色は、標準の Background プロパティを使用して設定でき、重大度によって設定される色は上書きされます。
独自の色を設定するときは、コンテンツの読みやすさとアクセシビリティを念頭に置いてください。


カスタム アイコンは、IconSource プロパティを使用して設定できます。 既定では、アイコンは表示されます (コントロールが折りたたまれていない場合)。
このアイコンは、IsIconVisible プロパティを false に設定することによって削除できます。 カスタム アイコンの場合、推奨されるアイコンのサイズは 20px です。


```xaml
<muxc:InfoBar x:Name="CallRecordingNotification"
    Title="Recording started"
    Message="Your call has begun recording."
    Background="{StaticResource LavenderBackgroundBrush}">
    <muxc:InfoBar.IconSource>
        <muxc:SymbolIconSource Symbol="Phone" />
    </muxc:InfoBar.IconSource>
</muxc:InfoBar>
```

![カスタムの背景色、カスタム アイコン、閉じるボタンが表示されている既定状態の InfoBar のサンプル](images/infobar-custom-icon-color.png)

### <a name="add-an-action-button"></a>アクション ボタンを追加する

[ButtonBase](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) を継承する独自のボタンを定義し、ActionButton プロパティでそれを設定することにより、アクション ボタンを追加できます。 一貫性とアクセシビリティのため、[Button](/uwp/api/Windows.UI.Xaml.Controls.Button) および [HyperlinkButton](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 型のアクション ボタンには、カスタム スタイル設定が適用されます。 ActionButton プロパティとは別に、カスタム コンテンツを使用してアクション ボタンを追加することができ、それはメッセージの下に表示されます。


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Network Settings" Click="InternetInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![1 行のメッセージとアクション ボタンが表示されて一ル、エラー状態の InfoBar のサンプル](images/infobar-error-action-button.png)

```xaml
<muxc:InfoBar x:Name="TermsAndConditionsUpdatedNotification"
    Title="Terms and Conditions Updated"
    Message="Dismissal of this message implies agreement to the updated Terms and Conditions. Please view the changes on our website.">
    <muxc:InfoBar.ActionButton>
        <HyperlinkButton Content="Terms and Conditions Sep 2020" 
            NavigateUri="https://www.example.com"
            Click="UpdateInfoBarHyperlinkButton_Click" />
    <muxc:InfoBar.ActionButton />
</muxc:InfoBar>
```
![複数行に展開されたメッセージとハイパーリンクが表示されている InfoBar のサンプル](images/infobar-default-hyperlink.png)

### <a name="content-wrapping"></a>コンテンツの折り返し
カスタム コンテンツを除く InfoBar のコンテンツを横 1 行に収めることができない場合は、垂直方向にレイアウトされます。 Title、Message、ActionButton (ある場合) は、それぞれ別の行に表示されます。


```xaml
<muxc:InfoBar x:Name="BackupCompleteNotification"
    Severity="Success"
    Title="Backup complete: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."  
    Message="All documents were uploaded successfully. Ultrices sagittis orci a scelerisque. Aliquet risus feugiat in ante metus dictum at tempor commodo. Auctor augue mauris augue neque gravida.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Action"
            Click="BackupInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![成功状態の複数行のタイトルとメッセージが表示された InfoBar のサンプル](images/infobar-success-content-wrapping.png)


### <a name="custom-content"></a>カスタム コンテンツ
Content プロパティを使用して、XAML コンテンツを InfoBar に追加できます。 それは、他のコントロール コンテンツの下の独自の行に表示されます。 InfoBar は、定義されているコンテンツに合わせて拡張されます。


```xaml
<muxc:InfoBar x:Name="BackupInProgressNotification"
    Title="Backup in progress"  
    Message="Your documents are being saved to the cloud"
    IsClosable="False">
    <muxc:InfoBar.Content>
        <muxc:ProgressBar IsIndeterminate="True" Margin="0,0,0,6" MaxWidth="200"/>
    </muxc:InfoBar.Content>
</muxc:InfoBar>
```

![不確定の進行状況バーが表示された既定の状態の InfoBar のサンプル](images/infobar-default-custom-content.gif)

### <a name="lightweight-styling"></a>軽量なスタイル設定

既定の Style と ControlTemplate を変更して、コントロールに固有の外観を与えることができます。 利用可能なテーマ リソースの一覧については、InfoBar API ドキュメントの「[コントロール スタイルとテンプレート](/windows/winui/api/microsoft.ui.xaml.controls.infobar#control-style-and-template)」を参照してください。
詳細については、[スタイル設定コントロール](./xaml-styles.md)に関する記事の「[軽量なスタイル設定](./xaml-styles.md#lightweight-styling)」セクションを参照してください。 

たとえば、次のようにすると、ページ上のすべての情報 InfoBar の背景色が青になります。

```xaml
<Page.Resources>
    <x:SolidColorBrush x:Key="InfoBarInformationalSeverityBackgroundBrush" Color="LightBlue"></x:SolidColorBrush>
</Page.Resources>
```
### <a name="canceling-close"></a>クローズの取り消し
Closing イベントを使用して、InfoBar のクローズをキャンセルしたり延期したりすることができます。 これは InfoBar を開いたままにしたり、カスタム アクションの実行の時間を確保したりするために使用できます。 InfoBar のクローズがキャンセルされると、IsOpen は true に戻ります。 プログラムによるクローズもキャンセルできます。


```xaml
<muxc:InfoBar x:Name="UpdateAvailable"
    Title="Update Available"
    Message="Please close this tip to apply required security updates to this application"
    Closing="InfoBar_Closing">
</muxc:InfoBar>
```

```csharp
public void InfoBar_Closing(InfoBar sender, InfoBarClosingEventArgs args)
{
    if (args.Reason == InfoBarCloseReason.CloseButton) {
        if (!ApplyUpdates()) {
            // could not apply updates - do not close
            args.Cancel = true;
        }
    }   
}
```

## <a name="recommendations"></a>推奨事項

### <a name="enter-and-exit-usability"></a>表示と消去の使いやすさ
#### <a name="flashing-content"></a>コンテンツの点滅
画面の点滅を防ぐため、InfoBar をビューに表示してすぐに消去しないようにする必要があります。 光過敏性のユーザーのため、そしてアプリケーションの使いやすさを向上させるため、ビジュアルが点滅しないようにします。


アプリの状態条件によりビューに自動的に表示されて消去される InfoBar の場合、コンテンツがすばやく表示または消去されたり、連続して何回も表示されたりするのを防ぐためのロジックを、アプリケーションに組み込むことをお勧めします。 ただし、一般に、このコントロールは表示期間の長い状態メッセージに使用する必要があります。


#### <a name="updating-the-infobar"></a>InfoBar の更新
コントロールが開かれた後、メッセージや重大度の更新など、さまざまなプロパティが変更されても、通知イベントは発生しません。 InfoBar の更新されたコンテンツのスクリーン リーダーを使用しているユーザーに通知する場合は、コントロールをいったん閉じてから再度開き、イベントをトリガーすることをお勧めします。


#### <a name="inline-messages-offsetting-content"></a>コンテンツをオフセットするインライン メッセージ
他の UI コンテンツとインラインで表示される InfoBar の場合、ページの残りの部分が要素の追加に応答してどのように反応するかに注意してください。


かなり高い InfoBar を使用すると、ページ上の他の要素のレイアウトが大幅に変更される可能性があります。 InfoBar がすばやく表示されたり消去されたりすると (特に連続して)、ユーザーは視覚状態の変化と混同する可能性があります。


### <a name="color-and-icon"></a>色とアイコン
事前設定された重大度レベル以外で色とアイコンをカスタマイズするときは、標準のアイコンと色のセットからの暗黙的な意味に関するユーザーの予想に注意してください。


また、事前設定された重大度の色は、テーマの変更、ハイコントラスト モード、色の混乱のユーザー補助、および前景色とのコントラストに関して既に設計されています。 可能な限りこれらの色を使用し、さまざまな色の状態とユーザー補助機能に適合するように、アプリケーションにカスタム ロジックを含めることをお勧めします。


[標準アイコン](/windows/win32/uxguide/vis-std-icons)と[色](/windows/win32/uxguide/vis-color)に関する UX ガイダンスを参照して、メッセージが明確かつわかりやすくユーザーに伝えられていることを確認してください。


#### <a name="severity"></a>重大度
 タイトル、メッセージ、またはカスタム コンテンツで伝達される情報と一致しない通知の場合に、Severity プロパティを設定しないでください。
 

 付随する情報は、その重大度を使用するために次の情報を伝達することを目的としたものである必要があります。
 - エラー:発生したエラーまたは問題。
 - 警告:将来の問題の原因となる可能性のある状態。
 - 成功: 実行時間の長いタスクまたはバックグラウンド タスクが完了した。
 - 既定:ユーザーの注意が必要な一般的な情報。

通知の意味を示す UI コンポーネントが、アイコンと色だけであってはなりません。 通知のタイトルやメッセージのテキストを使用して、情報を表示する必要があります。


### <a name="message"></a>Message 

通知内のテキストは、すべての言語で一定の長さではありません。 Title と Message プロパティの場合、通知が 2 行目に拡張されるかどうかに影響する可能性があります。 メッセージの長さや他の UI 要素が特定の言語に設定されることに基づいて配置しないようにすることをお勧めします。

右から左 (RTL) または左から右 (LTR) の言語との間でローカライズする場合、通知は標準的なミラーリングの動作に従います。 アイコンは、方向性がある場合にのみミラー化されます。

通知でのテキストのローカライズの詳細については、[レイアウトとフォントの調整および RTL のサポート](/uwp/design/globalizing/adjust-layout-and-fonts--and-support-rtl)に関するガイダンスを参照してください。

## <a name="related-articles"></a>関連記事

_ [ダイアログとポップアップ](./dialogs-and-flyouts/index.md)