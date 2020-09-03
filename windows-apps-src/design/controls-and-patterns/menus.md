---
Description: メニューとコンテキスト メニューは、ユーザーが要求するときにコマンドやオプションの一覧を表示します。
title: メニューとショートカット メニュー
label: Menus and context menus
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: RS5, 19H1
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: fab3cf710b50d3c8d643b3036eb1589a0fc113e3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172566"
---
# <a name="menus-and-context-menus"></a>メニューとショートカット メニュー

メニューとコンテキスト メニューは、ユーザーが要求するときにコマンドやオプションの一覧を表示します。 1 つのインライン メニューを表示するには、メニュー ポップアップを使います。 横一列 (通常はアプリ ウィンドウの上部) に複数のメニューを表示するには、メニュー バーを使います。 各メニューにはメニュー項目とサブメニューが含まれることがあります。

![一般的なコンテキスト メニューの例](images/contextmenu_rs2_icons.png)

**Windows UI ライブラリを入手する**

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | **MenuBar** コントロールは、Windows アプリのための新しいコントロールと UI 機能を含む NuGet パッケージである Windows UI ライブラリの一部として含まれています。 インストール手順などの詳細については、[Windows UI ライブラリの概要](/uwp/toolkits/winui/)に関するページを参照してください。 |

> **Windows UI ライブラリ API:** [MenuBar クラス](/uwp/api/microsoft.ui.xaml.controls.menubar)
>
> **プラットフォーム API:** [MenuFlyout クラス](/uwp/api/windows.ui.xaml.controls.menuflyout)、[MenuBar クラス](/uwp/api/windows.ui.xaml.controls.menubar)、[ContextFlyout プロパティ](/uwp/api/windows.ui.xaml.uielement.contextflyout)、[FlyoutBase.AttachedFlyout プロパティ](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

メニューとコンテキスト メニューは、コマンドを整理してユーザーに要求されるまで非表示にすることによって、スペースを節約します。 特定のコマンドを頻繁に使っていて、利用可能なスペースがある場合は、メニューを使って移動しなくてもよいように、メニュー内ではなく、独自の要素に直接配置することを検討してください。

メニューとコンテキスト メニューは、コマンドを整理する目的で使います。通知や確認要求などの任意のコンテンツを表示する場合は、[ダイアログまたはポップアップ](./dialogs-and-flyouts/index.md)を使います。

### <a name="menubar-vs-menuflyout"></a>MenuBar とMenuFlyout

キャンバス上の UI 要素にアタッチされたポップアップにメニューを表示するには、MenuFlyout コントロールを使ってメニュー項目をホストします。 メニュー ポップアップは、通常のメニューまたはコンテキスト メニューとして呼び出すことができます。 メニュー ポップアップは1 つのトップレベル メニュー (とオプションのサブメニュー) をホストします。

横一列に複数のトップレベル メニューを表示するには、メニュー バーを使います。 通常、メニュー バーはアプリ ウィンドウの上部に配置します。

### <a name="menubar-vs-commandbar"></a>MenuBar とCommandBar

MenuBar と CommandBar はどちらも、ユーザーにコマンドを公開するために使用できるサーフェスを表します。 MenuBar を使うと、CommandBar で許容されるよりも多くの組織またはグループ化が必要になる可能性があるアプリ用のコマンド セットをすばやく簡単に公開できます。

MenuBar を CommandBar と組み合わせて使うこともできます。 MenuBar は、複数のコマンドを提供するために使い、CommandBar は、最も使用されるコマンドを強調表示するために使います。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/MenuFlyout">アプリを開き、MenuFlyout の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>メニューとコンテキスト メニュー

メニューとコンテキスト メニューは、外観や、何を含めることができるかという点で似ています。 実際、これらは [MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout) という同じコントロールを使って作成できます。 違いは、ユーザーのアクセス方法です。

メニューまたはコンテキスト メニューは、どのような場合に使えばよいでしょうか。

- ホスト要素がボタンである場合や、追加のコマンドを表示することを主な役割とする他のコマンド要素である場合は、メニューを使います。
- ホスト要素が、別の主な役割 (テキストまたは画像を表示するなど) を持つ他の種類の要素である場合は、コンテキスト メニューを使います。

たとえば、ボタンのメニューを使って、リストのフィルター処理および並べ替えのオプションを提供します。 このシナリオでは、ボタン コントロールの主な役割は、メニューへのアクセスを提供することです。

![[メール] メニューの例](images/Mail_Menu.png)

テキスト要素にコマンド (切り取り、コピー、貼り付けなど) を追加する場合は、メニューの代わりにコンテキスト メニューを使います。 このシナリオでは、テキスト要素の主な役割はテキストを表示して編集することであり、追加のコマンド (切り取り、コピー、貼り付けなど) は補助的な役割であるため、コンテキスト メニューに属します。

![フォト ギャラリーでのコンテキスト メニューの例](images/ContextMenu_example.png)

### <a name="menus"></a>メニュー

- 常に表示される 1 つのエントリ ポイント (たとえば、画面上部の [ファイル] メニュー) があります。
- 通常、ボタンまたは親のメニュー項目にアタッチされます。
- 左クリック (または、指でタップするなどの同等の操作) によって呼び出されます。
- [Flyout](/uwp/api/windows.ui.xaml.controls.button.flyout) プロパティまたは [FlyoutBase.AttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) プロパティを介して要素に関連付けられます。また、アプリ ウィンドウの上部のメニュー バーにグループ化されます。

### <a name="context-menus"></a>コンテキスト メニュー

- 1 つの要素にアタッチされ、セカンダリ コマンドを表示します。
- 右クリック (または、指で長押しするなどの同等の操作) によって呼び出されます。
- [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) プロパティを介して要素に関連付けられます。

## <a name="icons"></a>アイコン

次のようなメニュー項目のアイコンを用意することを検討します。

- 最もよく使われる項目。
- アイコンが一般的またはよく知られているメニュー項目。
- アイコンがコマンドの役割を適切に示すメニュー項目。

標準的な視覚表現がないコマンドにアイコンを用意しなければならないと考える必要はありません。 わかりづらいアイコンは役に立たず、視覚的な混乱をもたらし、ユーザーが重要なメニュー項目に集中できなくなります。

![アイコンのあるコンテキスト メニューの例](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````

> [!TIP]
> MenuFlyoutItems のアイコンのサイズは 16 x 16 ピクセルです。 SymbolIcon、FontIcon、または PathIcon を使用した場合、忠実さを失うことなく、アイコンが適切なサイズに自動的に拡大縮小されます。 BitmapIcon を使用すると、アセットは必ず 16 x 16 ピクセルになります。  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>ポップアップ メニューまたはコンテキスト メニューの作成

ポップアップ メニューまたはコンテキスト メニューを作成するには、[MenuFlyout クラス](/uwp/api/windows.ui.xaml.controls.menuflyout)を使います。 メニューのコンテンツを定義するには、[MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.menuflyoutitem)、[MenuFlyoutSubItem](/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem)、[ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)、[RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)、[MenuFlyoutSeparator](/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) の各オブジェクトを MenuFlyout に追加します。

これらのオブジェクトの用途を次に説明します。

- [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.menuflyoutitem)—即座にアクションを実行します。
- [MenuFlyoutSubItem](/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem) - メニュー項目のカスケード リストを含みます。
- [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)—オプションのオンとオフを切り替えます。
- [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) - 相互に排他的なメニュー項目間を切り替えます。
- [MenuFlyoutSeparator](/uwp/api/windows.ui.xaml.controls.menuflyoutseparator)—メニュー項目を視覚的に分割します。

この例では、[MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout) を作成し、ほとんどのコントロールで利用できる [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) プロパティを使って、コンテキスト メニューとして MenuFlyout を表示します。

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

次の例はほとんど同じですが、[ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) プロパティを使って、コンテキスト メニューとして [MenuFlyout クラス](/uwp/api/windows.ui.xaml.controls.menuflyout)を表示する代わりに、[FlyoutBase.ShowAttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) プロパティを使って、メニューとして MenuFlyout クラスを表示します。

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

### <a name="light-dismiss"></a>簡易非表示

簡易非表示コントロール (メニュー、コンテキスト メニュー、その他のポップアップ) は、閉じられるまで一時的な UI にキーボードのフォーカスやゲームパッドのフォーカスを捕捉します。 この動作に視覚的な合図を提供するために、Xbox の簡易非表示コントロールは、スコープ外の UI を暗く表示するオーバーレイを描画します。 この動作は、[LightDismissOverlayMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode) プロパティを使って変更できます。 既定で、一時的な UI によって Xbox 上では簡易非表示オーバーレイが描画されます (**自動**) が、他のデバイス ファミリ上では描画されません。 オーバーレイは、常に **On** にするか、常に **Off** にするかを選択できます。

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>メニュー バーの作成

> [!IMPORTANT]
> MenuBar には、Windows 10 Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 以降、または [Windows UI ライブラリ](/uwp/toolkits/winui/)が必要です。

メニュー ポップアップと同じ要素を使ってメニュー バーにメニューを作成します。 ただし、MenuFlyoutItem オブジェクトは MenuFlyout でグループ化せずに、MenuBarItem 要素でグループ化します。 各 MenuBarItem はトップ レベル メニューとして MenuBar に追加されます。

![メニュー バーの例](images/menu-bar-submenu.png)

> [!NOTE]
> この例は、UI 構造の作成方法のみを示していますが、どのコマンドの実装も示していません。

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。
- [XAML コンテキスト メニューのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>関連記事

- [MenuFlyout クラス](/uwp/api/windows.ui.xaml.controls.menuflyout)
- [MenuBar クラス](/uwp/api/microsoft.ui.xaml.controls.menubar)