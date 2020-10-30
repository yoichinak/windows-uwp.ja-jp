---
description: ダイアログとポップアップは、ユーザーが要求したとき、または通知や許可を必要とする状況が発生したときに表示される一時的な UI 要素です。
title: ダイアログとポップアップ
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: bf4746a6d3a024fec31045bbf9c63d3b11315b8c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033845"
---
# <a name="dialogs-and-flyouts"></a>ダイアログとポップアップ

ダイアログ ボックスとポップアップは、通知、許可、またはユーザーからの追加の情報を必要とする状況が発生したときに表示される一時的な UI 要素です。

> **プラットフォーム API:** [ContentDialog クラス](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、[Flyout クラス](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

**ダイアログ**

![ダイアログの例](../images/dialogs/dialog_RS2_delete_file.png)

ダイアログは、状況依存のアプリ情報を表示するモーダル UI オーバーレイです。 ダイアログは、明示的に閉じられるまでアプリ ウィンドウの対話式操作をブロックします。 多くの場合、ユーザーに何らかの操作を要求します。

**ポップアップ**

![ポップアップの例](../images/flyout-example2.png)

ポップアップ (flyout) は、ユーザーが現在操作している内容に関係する UI を表示する軽量な状況依存のポップアップです。 このポップアップは、配置ロジックとサイズ設定ロジックを備えており、セカンダリ コントロールを表示する場合、または項目の詳細を表示する場合に使うことができます。

ダイアログとは異なり、ポップアップは、それ以外の場所をタップまたはクリックするか、Esc キーまたは戻るボタンを押すか、アプリ ウィンドウのサイズを変更するか、デバイスの向きを変更することで、すばやく閉じることができます。

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

ダイアログとポップアップにより、ユーザーが重要な情報を認識していることを確認できますが、ユーザー エクスペリエンスは中断されます。 ダイアログはモーダル (ブロック) であるため、ユーザーは中断され、ダイアログの操作を行うまで他の操作を行うことはできません。 ポップアップの煩わしさはダイアログより低くなりますが、多用すると、煩わしくなります。

ダイアログかポップアップを使用すると決めた場合には、どちらを選択する必要があります。

ダイアログは操作をブロックし、ポップアップはブロックしないため、ダイアログの使用は、ユーザーが他のすべてを中断して情報や回答の提供に集中する必要がある状況に限定する必要があります。 一方ポップアップは、ユーザーに情報を知らせるが、ユーザーがそれを無視してもよい場合に使用します。

   <p><b>ダイアログの用途</b> <br/>
<ul>
<li>続行前にユーザーが読んだり確認したりする<b>必要のある重要な</b>情報を表示する場合。 たとえば、次のようになります。
<ul>
  <li>ユーザーのセキュリティが侵害される可能性がある場合</li>
  <li>ユーザーが重要な資産に永続的な変更を加えようとしている場合</li>
  <li>ユーザーが重要な資産を削除しようとしている場合</li>
  <li>アプリ内購入を確認する場合</li>
</ul>

</li>
<li>接続エラーなど、アプリ全体の状況に適用されるエラー メッセージ</li>
<li>アプリからユーザーにブロック質問を表示する必要がある場合 (アプリで自動的に選ぶことができない場合など) ブロック質問とは、無視したり先送りにしたりできない質問です。この質問では、ユーザーに明確な選択肢を提示する必要があります。</li>
</ul>
</p>


   <p><b>ポップアップの用途</b> <br/>
<ul>
<li>操作を完了する前に、必要な追加情報を収集する場合。</li>
<li>一部の場合のみに意味がある情報を表示する場合。 たとえばフォト ギャラリー アプリで、ユーザーが画像のサムネイルをクリックした場合に、大きな画像を表示するためにポップアップを使用できます。</li>
<li>詳細やページ上の項目の長い説明などの詳しい情報の表示。</li>
</ul></p>

## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>ダイアログとポップアップを使用しないようにする方法

伝える情報の重要度が、ユーザーを中断させる必要があるものかどうかを、よく検討する必要があります。 また、情報の表示頻度を検討し、数分ごとにダイアログや通知を表示している場合には、代わりにプライマリ UI でこの情報用の領域を割り当てることを検討します。 たとえばチャット クライアントで、友人がログインするたびにポップアップを表示させるよりも、その時点でオンラインである友人の一覧を表示し、ログインが行われたときには強調表示させるなどの方法を検討します。

ダイアログは、アクション (ファイルの削除など) を実行する前に確認するために、よく使用されます。 ユーザーが特定の操作を頻繁に実行することが想定される場合には、ユーザーがアクションを毎回確認する必要があるようにするよりも、誤って操作した場合に、ユーザーが元に戻せる方法を提供することを検討します。

## <a name="how-to-create-a-dialog"></a>ダイアログの作成方法

[ダイアログに関する記事](dialogs.md)を参照してください。 

## <a name="how-to-create-a-flyout"></a>ポップアップの作成方法

[ポップアップに関する記事](flyouts.md)を参照してください。 

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックしてアプリを開き、<a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> または <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> の動作を確認してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

